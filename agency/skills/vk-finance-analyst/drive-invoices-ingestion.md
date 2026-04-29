# Скилл: drive-invoices-ingestion

> **Агент:** vk-finance-analyst
> **Назначение:** Забрать Накладные (PDF) из Drive, извлечь COGS (себестоимость закупки), передать в Finance View для расчёта реальной маржи

---

## Зачем

Продажи и Выручка из Сабы содержат расчётную себестоимость (по нормам). Накладные = фактические затраты на закупку у поставщиков. Разница между расчётной и фактической себестоимостью — это и есть реальная маржа.

Без Накладных: `маржа = расчётная (Саби)`
С Накладными: `маржа = фактическая (поставщик)`

---

## Источник в Drive

| Тип файла | Папка | Паттерн имени |
|-----------|-------|--------------|
| Накладные поставщиков (PDF) | Накладные: `15QhRzYuS0AgVanaccz-SF-C_kiqReVd8` | `Накладные_YYYY-MM-DD_YYYY-MM-DD.pdf` |

Ссылка на папку: `https://drive.google.com/drive/folders/15QhRzYuS0AgVanaccz-SF-C_kiqReVd8`

Папка-inbox (новые файлы от Жанны): `https://drive.google.com/drive/folders/1Jg1Zgj2_ueTV6-NQamAU8XCPalFNhO8W`

---

## Предусловия

- `pdfplumber` установлен: `pip install pdfplumber`
- Авторизация Drive: `VK-offee/.github/scripts/token.pickle`
- Скрипт загрузки: `VK-offee/saby-integration/google_drive_parser.py`

---

## Алгоритм

### 1. Найти и скачать последние Накладные

```python
import sys
sys.path.insert(0, '/Users/alexander/Github/VK-offee/saby-integration')
from google_drive_parser import GoogleDriveParser

parser = GoogleDriveParser(
    credentials_path='/Users/alexander/Github/VK-offee/.github/scripts/credentials.json',
    token_path='/Users/alexander/Github/VK-offee/.github/scripts/token.pickle'
)

files = parser.list_files('1sGGcG1DBHIMMhZFvPGd_gGOesncQwhiq')
invoice_files = [f for f in files if f['name'].startswith('Накладные_') and f['name'].endswith('.pdf')]

# Найти файл с максимальной конечной датой
import re
from datetime import datetime

def get_latest_invoice(files):
    pattern = r'Накладные_(\d{4}-\d{2}-\d{2})_(\d{4}-\d{2}-\d{2})\.pdf'
    candidates = []
    for f in files:
        m = re.match(pattern, f['name'])
        if m:
            end_date = datetime.strptime(m.group(2), '%Y-%m-%d')
            candidates.append((end_date, f))
    return sorted(candidates, reverse=True)[0][1] if candidates else None

latest = get_latest_invoice(invoice_files)
if latest:
    data_layer = '/Users/alexander/Github/DS-finance-private/business-finance/data-layer'
    parser.download_file(latest['id'], f"{data_layer}/{latest['name']}")
```

### 2. Извлечь таблицы из PDF

```python
import pdfplumber

def parse_invoice_pdf(pdf_path):
    """
    Саби генерирует структурированные PDF с таблицами.
    Извлекаем: позиция, количество, цена закупки, сумма, поставщик.
    """
    rows = []
    with pdfplumber.open(pdf_path) as pdf:
        for page in pdf.pages:
            tables = page.extract_tables()
            for table in tables:
                if not table:
                    continue
                headers = [str(h).lower().strip() if h else '' for h in table[0]]
                for row in table[1:]:
                    if not any(row):
                        continue
                    rows.append(dict(zip(headers, row)))
    return rows
```

### 3. Нормализовать COGS

Ожидаемые колонки Сабы в накладной:
- `Номенклатура` — название позиции
- `Количество` — кол-во единиц
- `Цена` — цена закупки за единицу ₽
- `Сумма` — итого по позиции ₽
- `Поставщик` / `Контрагент` — название поставщика
- `Дата` — дата накладной

```python
def normalize_invoice_rows(raw_rows):
    """Нормализовать строки накладной в единый формат."""
    normalized = []
    for row in raw_rows:
        # Сопоставление колонок (Саби может менять порядок)
        name = row.get('номенклатура') or row.get('наименование') or row.get('товар', '')
        qty = _parse_number(row.get('количество') or row.get('кол-во', '0'))
        price = _parse_number(row.get('цена', '0'))
        amount = _parse_number(row.get('сумма', '0'))
        supplier = row.get('поставщик') or row.get('контрагент', '')
        date = row.get('дата', '')

        if name and amount:
            normalized.append({
                'name': name.strip(),
                'qty': qty,
                'price': price,
                'amount': amount,
                'supplier': supplier.strip(),
                'date': date,
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

### 4. Агрегировать COGS по периоду

```python
def aggregate_cogs(normalized_rows, period_start, period_end):
    """Итоговые затраты на закупку за период."""
    total_cogs = sum(r['amount'] for r in normalized_rows)
    by_supplier = {}
    for row in normalized_rows:
        sup = row['supplier'] or 'неизвестен'
        by_supplier[sup] = by_supplier.get(sup, 0) + row['amount']

    return {
        'period_start': period_start,
        'period_end': period_end,
        'total_cogs': total_cogs,
        'by_supplier': by_supplier,
        'items_count': len(normalized_rows),
    }
```

---

## Использование COGS в Finance View

После получения `total_cogs` — передать в `finance-view-builder.md`:

```python
# Реальная маржа = (выручка - фактические COGS) / выручка * 100
real_margin_pct = round((total_revenue - total_cogs) / total_revenue * 100, 1)
```

Отличие от расчётной маржи Сабы фиксировать явно:

```markdown
## Маржа
- Расчётная (Саби): NN%
- Фактическая (Накладные): NN%
- Расхождение: ±N п.п.
```

---

## Обработка ошибок

| Ситуация | Действие |
|----------|----------|
| PDF не найден в Drive | Статус partial, маржа = расчётная из Сабы |
| Таблица не извлечена | Логировать страницу, пропустить, указать в статусе |
| Колонки не распознаны | Вывести первую строку для диагностики, статус partial |
| Сумма COGS = 0 | Считать данные невалидными, не использовать в Finance View |

---

## Выход скилла

```python
invoice_result = {
    'status': 'done' | 'partial',
    'period_start': 'YYYY-MM-DD',
    'period_end': 'YYYY-MM-DD',
    'total_cogs': 850000.0,      # фактические затраты на закупку ₽
    'by_supplier': {             # разбивка по поставщикам
        'ООО Кофе Трейд': 420000,
        'ИП Иванов': 430000,
    },
    'items_count': 142,
    'source_file': 'Накладные_2026-03-30_2026-04-14.pdf',
}
```
