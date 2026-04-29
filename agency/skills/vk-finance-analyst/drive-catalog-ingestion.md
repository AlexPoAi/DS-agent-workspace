# Скилл: drive-catalog-ingestion

> **Агент:** vk-finance-analyst
> **Назначение:** Забрать Каталог (номенклатура Excel) из Drive, извлечь цены и позиции, передать в unit-экономику

---

## Зачем

Каталог из Сабы — это полный список номенклатуры с ценами продажи (и возможно ценами закупки). Используется для:

- расчёта маржи по SKU (какие позиции прибыльны, какие убыточны);
- unit-экономики: средний чек, структура продаж;
- сравнения цены продажи с фактической закупочной ценой (из Накладных).

---

## Источник в Drive

| Тип файла | Папка | Паттерн имени |
|-----------|-------|--------------|
| Каталог номенклатуры (Excel) | Каталог: `1QEQcMso75T5WJt3N4bEbFwl7_6e6vLJb` | `Каталог_YYYY-MM-DD.xlsx` |

Ссылка на папку: `https://drive.google.com/drive/folders/1QEQcMso75T5WJt3N4bEbFwl7_6e6vLJb`

---

## Предусловия

- `openpyxl` установлен: `pip install openpyxl`
- Авторизация Drive: `VK-offee/.github/scripts/token.pickle`
- Скрипт загрузки: `VK-offee/saby-integration/google_drive_parser.py`

---

## Алгоритм

### 1. Найти и скачать последний Каталог

```python
import sys, re
from datetime import datetime
sys.path.insert(0, '/Users/alexander/Github/VK-offee/saby-integration')
from google_drive_parser import GoogleDriveParser

parser = GoogleDriveParser(
    credentials_path='/Users/alexander/Github/VK-offee/.github/scripts/credentials.json',
    token_path='/Users/alexander/Github/VK-offee/.github/scripts/token.pickle'
)

files = parser.list_files('1sGGcG1DBHIMMhZFvPGd_gGOesncQwhiq')
catalog_files = [f for f in files if f['name'].startswith('Каталог_') and f['name'].endswith('.xlsx')]

def get_latest_catalog(files):
    pattern = r'Каталог_(\d{4}-\d{2}-\d{2})\.xlsx'
    candidates = []
    for f in files:
        m = re.match(pattern, f['name'])
        if m:
            date = datetime.strptime(m.group(1), '%Y-%m-%d')
            candidates.append((date, f))
    return sorted(candidates, reverse=True)[0][1] if candidates else None

latest = get_latest_catalog(catalog_files)
if latest:
    data_layer = '/Users/alexander/Github/DS-finance-private/business-finance/data-layer'
    parser.download_file(latest['id'], f"{data_layer}/{latest['name']}")
```

### 2. Прочитать номенклатуру

```python
import openpyxl

def parse_catalog(xlsx_path):
    """
    Каталог Сабы: список позиций с ценами.
    Ожидаемые колонки: Номенклатура, Цена продажи, Единица, Группа.
    """
    wb = openpyxl.load_workbook(xlsx_path, data_only=True)
    ws = wb.active

    headers = None
    rows = []
    for i, row in enumerate(ws.iter_rows(values_only=True)):
        if i == 0:
            headers = [str(h).lower().strip() if h else f'col_{i}' for i, h in enumerate(row)]
            continue
        if not any(row):
            continue
        rows.append(dict(zip(headers, row)))

    return rows
```

### 3. Нормализовать позиции

Ожидаемые колонки Сабы:
- `Номенклатура` / `Наименование` — название позиции
- `Цена` / `Цена продажи` — розничная цена ₽
- `Себестоимость` / `Цена закупки` — закупочная цена ₽ (если есть)
- `Единица` / `Ед. изм.` — единица измерения
- `Группа` / `Категория` — категория (напитки, еда, товары)

```python
def normalize_catalog_rows(raw_rows):
    normalized = []
    for row in raw_rows:
        name = (row.get('номенклатура') or row.get('наименование') or '').strip()
        price_sale = _parse_number(row.get('цена продажи') or row.get('цена', '0'))
        price_cost = _parse_number(row.get('себестоимость') or row.get('цена закупки', '0'))
        unit = (row.get('единица') or row.get('ед. изм.') or '').strip()
        group = (row.get('группа') or row.get('категория') or '').strip()

        if name and price_sale:
            margin_pct = None
            if price_cost > 0:
                margin_pct = round((price_sale - price_cost) / price_sale * 100, 1)

            normalized.append({
                'name': name,
                'price_sale': price_sale,
                'price_cost': price_cost,
                'margin_pct': margin_pct,
                'unit': unit,
                'group': group,
            })

    return normalized

def _parse_number(val):
    if val is None:
        return 0.0
    cleaned = str(val).replace(' ', '').replace(',', '.').replace('\xa0', '')
    try:
        return float(cleaned)
    except ValueError:
        return 0.0
```

### 4. Агрегировать по группам

```python
from collections import defaultdict

def aggregate_by_group(normalized_rows):
    """Сводка по категориям: кол-во позиций, средняя маржа."""
    by_group = defaultdict(list)
    for row in normalized_rows:
        by_group[row['group'] or 'без категории'].append(row)

    summary = {}
    for group, items in by_group.items():
        margins = [r['margin_pct'] for r in items if r['margin_pct'] is not None]
        summary[group] = {
            'items_count': len(items),
            'avg_margin_pct': round(sum(margins) / len(margins), 1) if margins else None,
            'price_range': (
                min(r['price_sale'] for r in items),
                max(r['price_sale'] for r in items),
            ),
        }

    return summary
```

---

## Использование в unit-экономике

Каталог + Продажи = полная картина по SKU:

```python
# Соединить каталог с данными продаж
# Продажи дают: кол-во продаж по позиции
# Каталог дает: цена продажи, маржа
# Результат: выручка и маржа по каждой позиции за период
```

Это задача `business_unit_agent` — vk-finance-analyst передаёт нормализованный каталог туда.

---

## Обработка ошибок

| Ситуация | Действие |
|----------|----------|
| Каталог не найден | Работать без него, unit-экономика = partial |
| Колонка цены не найдена | Вывести список колонок для диагностики |
| Цена закупки отсутствует | Маржа по SKU = partial (только от Накладных) |

---

## Выход скилла

```python
catalog_result = {
    'status': 'done' | 'partial',
    'catalog_date': 'YYYY-MM-DD',
    'items_total': 234,
    'items': [
        {'name': 'Капучино 300мл', 'price_sale': 250, 'price_cost': 45, 'margin_pct': 82.0, 'group': 'Напитки'},
        {'name': 'Боул с курицей', 'price_sale': 380, 'price_cost': 140, 'margin_pct': 63.2, 'group': 'Еда'},
    ],
    'by_group': {
        'Напитки': {'items_count': 45, 'avg_margin_pct': 79.3},
        'Еда': {'items_count': 28, 'avg_margin_pct': 61.1},
    },
    'source_file': 'Каталог_2026-04-14.xlsx',
}
```
