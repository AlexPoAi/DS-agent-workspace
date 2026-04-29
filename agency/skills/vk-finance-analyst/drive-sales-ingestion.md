# Скилл: drive-sales-ingestion

> **Агент:** vk-finance-analyst
> **Назначение:** Забрать последние Саби-отчёты из Google Drive, нормализовать, сохранить в data-layer

---

## Предусловия

Перед запуском проверить:
1. Существует `VK-offee/.github/scripts/token.pickle` (авторизация Drive API)
2. Существует `VK-offee/saby-integration/google_drive_parser.py`
3. Папка `DS-finance-private/business-finance/data-layer/` доступна для записи

---

## Папки Drive (зафиксированы)

| Тип данных | Folder ID | Ссылка |
|-----------|-----------|--------|
| Продажи | `1g2Pq-cYVT2cSkd9DwBCV-EJBHYDvzHw9` | [открыть](https://drive.google.com/drive/folders/1g2Pq-cYVT2cSkd9DwBCV-EJBHYDvzHw9) |
| Выручка | В той же папке или рядом — искать файлы `Выручка_*.xlsx` | |

> Карта всех папок Drive: `DS-strategy/exocortex/reference-google-drive-vkoffee.md`

---

## Алгоритм ingestion

### 1. Получить список файлов из Drive

```python
# Шаблон вызова через google_drive_parser.py
# Файл: VK-offee/saby-integration/google_drive_parser.py
# Credentials: VK-offee/.github/scripts/credentials.json
# Token: VK-offee/.github/scripts/token.pickle

import sys
sys.path.insert(0, '/Users/alexander/Github/VK-offee/saby-integration')
from google_drive_parser import GoogleDriveParser

parser = GoogleDriveParser(
    credentials_path='/Users/alexander/Github/VK-offee/.github/scripts/credentials.json',
    token_path='/Users/alexander/Github/VK-offee/.github/scripts/token.pickle'
)

# Листинг папки Продажи
files = parser.list_files('1g2Pq-cYVT2cSkd9DwBCV-EJBHYDvzHw9')
# Ожидаемые имена: Продажи_YYYY-MM-DD_YYYY-MM-DD.xlsx, Выручка_YYYY-MM-DD_YYYY-MM-DD.xlsx
```

### 2. Найти последние файлы

Приоритет выбора файла:
1. Самая поздняя дата окончания в имени файла (формат `_YYYY-MM-DD.xlsx` в конце)
2. Если несколько файлов с одной датой — взять наибольший по размеру
3. Скачивать ОБА типа: `Продажи_*.xlsx` и `Выручка_*.xlsx`

```python
import re
from datetime import datetime

def get_latest_file(files, prefix):
    """Найти файл с максимальной конечной датой в имени."""
    pattern = rf'{prefix}_(\d{{4}}-\d{{2}}-\d{{2}})_(\d{{4}}-\d{{2}}-\d{{2}})\.xlsx'
    candidates = []
    for f in files:
        m = re.match(pattern, f['name'])
        if m:
            end_date = datetime.strptime(m.group(2), '%Y-%m-%d')
            candidates.append((end_date, f))
    if not candidates:
        return None
    return sorted(candidates, key=lambda x: x[0], reverse=True)[0][1]

sales_file = get_latest_file(files, 'Продажи')
revenue_file = get_latest_file(files, 'Выручка')
```

### 3. Скачать файлы

```python
data_layer_path = '/Users/alexander/Github/DS-finance-private/business-finance/data-layer'

if sales_file:
    parser.download_file(sales_file['id'], f"{data_layer_path}/{sales_file['name']}")
    
if revenue_file:
    parser.download_file(revenue_file['id'], f"{data_layer_path}/{revenue_file['name']}")
```

### 4. Нормализовать данные

#### Структура файла Продажи (ожидаемые колонки Сабы):
- `Номенклатура` — название позиции
- `Количество` — штук продано
- `Сумма` — выручка ₽
- `Точка` / `Подразделение` / аналогичное — название точки

#### Структура файла Выручка:
- `Дата` — день
- `Точка` / `Организация` — название точки
- `Выручка` / `Сумма` — ₽
- `Себестоимость` / `Расход` — ₽ (если есть, для расчёта маржи)

#### Маппинг точек → slug:

```python
POINT_SLUG_MAP = {
    'тургенева': 'turgeneva',
    'тургенева 20': 'turgeneva',
    'карьерный': 'lugovaya',
    'карьерный переулок': 'lugovaya',
    'луговая': 'lugovaya',
    'самокиша': 'samokisha',
    'самокиша 5б': 'samokisha',
    'самокиша 5': 'samokisha',
}

def normalize_point(raw_name: str) -> str:
    normalized = raw_name.lower().strip()
    for key, slug in POINT_SLUG_MAP.items():
        if key in normalized:
            return slug
    return raw_name.lower().replace(' ', '_')  # fallback
```

#### Нормализованная структура (на выход):

```python
# daily_data: list of dicts
{
    'date': 'YYYY-MM-DD',
    'point': 'turgeneva',  # slug
    'revenue': 146000.0,   # выручка ₽
    'cost': 55800.0,       # себестоимость ₽ (если есть)
    'margin_pct': 61.8,    # (revenue - cost) / revenue * 100
    'source': 'Выручка_2026-03-30_2026-04-14.xlsx'
}
```

---

## Обработка ошибок

| Ситуация | Действие |
|----------|----------|
| Файл уже скачан (то же имя) | Пропустить скачивание, использовать существующий |
| Файл не найден в Drive | Статус partial, указать какого файла не хватает |
| Колонка `Точка` не найдена | Проверить альтернативные имена (`Подразделение`, `Организация`, `Объект`) |
| Точка не распознана | Логировать как `unknown_XXXX`, не терять данные |
| token.pickle просрочен | Сообщить пользователю: «Нужно обновить токен Drive» |

---

## Выход скилла

```python
ingestion_result = {
    'status': 'done',  # или 'partial'
    'period_start': 'YYYY-MM-DD',
    'period_end': 'YYYY-MM-DD',
    'files_downloaded': ['Продажи_*.xlsx', 'Выручка_*.xlsx'],
    'daily_data': [...],  # нормализованные строки
    'missing': [],        # что отсутствует (для partial)
}
```

Передать `ingestion_result` в скилл `finance-view-builder.md`.
