# Скилл: drive-sales-ingestion

> **Агент:** vk-finance-analyst
> **Назначение:** Забрать Саби-отчёты из Drive (Продажи + Выручка), нормализовать, переместить обработанные файлы в «Обработано»

---

## Жизненный цикл файлов (ВАЖНО)

```
Жанна загружает → [Новые документы]
                         ↓
          vk-finance-analyst читает и обрабатывает
                         ↓
          перемещает в [Обработано]
                         ↓
          нормализованные данные → data-layer → Finance View
```

**Правило:** обработанный файл ВСЕГДА перемещается из «Новые документы» в «Обработано». Файл в «Новые документы» = ещё не обработан.

---

## Папки Drive (зафиксированы)

| Папка | Folder ID | Ссылка |
|-------|-----------|--------|
| **Новые документы** (inbox) | `1Jg1Zgj2_ueTV6-NQamAU8XCPalFNhO8W` | [открыть](https://drive.google.com/drive/folders/1Jg1Zgj2_ueTV6-NQamAU8XCPalFNhO8W) |
| **Обработано** (archive) | `1WanukzWeuqJgUQ7N8YkG9oC23-DIRGjT` | [открыть](https://drive.google.com/drive/folders/1WanukzWeuqJgUQ7N8YkG9oC23-DIRGjT) |
| **Продажи** (тематическая) | `17Q7UI2mltbDlKoJrWE6dYchwIvux3mKU` | [открыть](https://drive.google.com/drive/folders/17Q7UI2mltbDlKoJrWE6dYchwIvux3mKU) |
| **Выручка** (тематическая) | `1E6aKSFEtLXnIk-8RMAc7fxp5nc2rWqly` | [открыть](https://drive.google.com/drive/folders/1E6aKSFEtLXnIk-8RMAc7fxp5nc2rWqly) |

> Карта всех папок: `DS-strategy/exocortex/reference-google-drive-vkoffee.md`

---

## Предусловия

- `VK-offee/.github/scripts/credentials.json` — credentials Drive API
- `VK-offee/saby-integration/google_drive_parser.py` — парсер

---

## Алгоритм ingestion

### 1. Инициализация

```python
import sys, re
from datetime import datetime
sys.path.insert(0, '/Users/alexander/Github/VK-offee/saby-integration')
from google_drive_parser import SabyGoogleDriveParser

parser = SabyGoogleDriveParser(
    credentials_file='/Users/alexander/Github/VK-offee/.github/scripts/credentials.json'
)
service = parser.service
```

### 2. Проверить «Новые документы» — есть ли необработанные файлы

```python
INBOX_ID = '1Jg1Zgj2_ueTV6-NQamAU8XCPalFNhO8W'
DONE_ID  = '1WanukzWeuqJgUQ7N8YkG9oC23-DIRGjT'

def list_folder(folder_id):
    return service.files().list(
        q=f"'{folder_id}' in parents",
        pageSize=100,
        fields="files(id, name, size, mimeType)"
    ).execute().get('files', [])

inbox_files = list_folder(INBOX_ID)
new_sales   = [f for f in inbox_files if f['name'].startswith('Продажи_')]
new_revenue = [f for f in inbox_files if f['name'].startswith('Выручка_')]

print(f"Новые документы: {len(new_sales)} Продажи, {len(new_revenue)} Выручка")
```

Если `inbox_files` пуст — взять из тематических папок (Продажи / Выручка) как fallback.

### 3. Выбрать файлы с максимальной датой

```python
def get_latest(files, prefix):
    """Файл с самой поздней датой окончания периода в имени."""
    pattern = rf'{prefix}_(\d{{4}}-\d{{2}}-\d{{2}})_(\d{{4}}-\d{{2}}-\d{{2}})\.xlsx'
    candidates = []
    for f in files:
        m = re.match(pattern, f['name'])
        if m:
            end_date = datetime.strptime(m.group(2), '%Y-%m-%d')
            candidates.append((end_date, f))
    if not candidates:
        return None, None, None
    _, best = sorted(candidates, reverse=True)[0]
    m = re.match(pattern, best['name'])
    return best, m.group(1), m.group(2)  # file, period_start, period_end

sales_file,   p_start, p_end = get_latest(new_sales or list_folder('17Q7UI2mltbDlKoJrWE6dYchwIvux3mKU'), 'Продажи')
revenue_file, _,       _     = get_latest(new_revenue or list_folder('1E6aKSFEtLXnIk-8RMAc7fxp5nc2rWqly'), 'Выручка')
```

### 4. Скачать файлы

```python
import os

DATA_LAYER = '/Users/alexander/Github/DS-finance-private/business-finance/data-layer'

def download_if_needed(f):
    if f is None:
        return None
    dest = os.path.join(DATA_LAYER, f['name'])
    if os.path.exists(dest):
        print(f"Уже есть локально: {f['name']}")
        return dest
    parser.download_file(f['id'], dest)
    print(f"Скачан: {f['name']}")
    return dest

sales_path   = download_if_needed(sales_file)
revenue_path = download_if_needed(revenue_file)
```

### 5. Нормализовать данные

#### Маппинг точек → slug

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
    s = raw_name.lower().strip()
    for key, slug in POINT_SLUG_MAP.items():
        if key in s:
            return slug
    return s.replace(' ', '_')
```

#### Нормализованная строка (выход)

```python
{
    'date': 'YYYY-MM-DD',
    'point': 'turgeneva',   # slug
    'revenue': 146000.0,    # выручка ₽
    'cost': 55800.0,        # себестоимость ₽ (если есть в Выручка.xlsx)
    'margin_pct': 61.8,     # (revenue - cost) / revenue * 100
    'source': 'Выручка_2026-03-30_2026-04-14.xlsx',
}
```

### 6. ⭐ Переместить обработанные файлы в «Обработано»

```python
def move_to_done(file_id, file_name, from_folder_id=INBOX_ID):
    """Переместить файл из inbox в Обработано."""
    service.files().update(
        fileId=file_id,
        addParents=DONE_ID,
        removeParents=from_folder_id,
        fields='id, parents'
    ).execute()
    print(f"Перемещён в Обработано: {file_name}")

# Перемещать только файлы из inbox (не из тематических папок)
if sales_file and sales_file in new_sales:
    move_to_done(sales_file['id'], sales_file['name'])

if revenue_file and revenue_file in new_revenue:
    move_to_done(revenue_file['id'], revenue_file['name'])
```

**Важно:** если файл взят из тематической папки (Продажи / Выручка) как fallback — не перемещать, он уже обработан ранее.

---

## Обработка ошибок

| Ситуация | Действие |
|----------|----------|
| Inbox пуст | Взять из тематических папок (Продажи / Выручка) как fallback |
| Файл уже скачан локально | Пропустить скачивание, использовать существующий |
| Колонка `Точка` не найдена | Проверить: `Подразделение`, `Организация`, `Объект` |
| Точка не распознана | Логировать как `unknown_XXXX`, не терять данные |
| Перемещение в Обработано упало | Логировать, не блокировать — данные уже скачаны |

---

## Режим: сбор всей истории для статистики

Если нужна динамика/тренды за несколько периодов — передать `all_history=True`.
Агент читает ВСЕ файлы из «Обработано» + тематических папок, не только последний.

```python
def collect_all_history(prefix):
    """Собрать все файлы заданного типа из Обработано и тематических папок."""
    all_files = []
    for folder_id in [DONE_ID, '17Q7UI2mltbDlKoJrWE6dYchwIvux3mKU',
                      '1E6aKSFEtLXnIk-8RMAc7fxp5nc2rWqly']:
        files = list_folder(folder_id)
        all_files += [f for f in files if f['name'].startswith(prefix)]

    # Дедупликация по имени
    seen = {}
    for f in all_files:
        seen[f['name']] = f
    return list(seen.values())

# Использование:
# if all_history:
#     all_sales = collect_all_history('Продажи_')
#     all_revenue = collect_all_history('Выручка_')
#     # скачать все, нормализовать все периоды, передать в finance-view-builder
```

Результат передаётся в `finance-view-builder.md` с флагом `multi_period=True` —
builder строит таблицу динамики по неделям/месяцам вместо одного периода.

---

## Выход скилла

```python
ingestion_result = {
    'status': 'done',      # или 'partial'
    'period_start': 'YYYY-MM-DD',
    'period_end': 'YYYY-MM-DD',
    'files_downloaded': ['Продажи_*.xlsx', 'Выручка_*.xlsx'],
    'files_moved_to_done': ['Продажи_*.xlsx'],  # что перемещено из inbox
    'daily_data': [...],   # нормализованные строки
    'missing': [],         # что не нашлось (для partial)
}
```

Передать `ingestion_result` в скилл `finance-view-builder.md`.
