---
skill: warehouse-pdf-line-item-intelligence
agent: warehouse-demand-analyst
created: 2026-04-21
status: active
---

# Warehouse PDF Line-Item Intelligence

## Цель

Разбирать накладные не до “preview text”, а до line-item ledger:
`supplier -> invoice -> date -> sku -> qty -> unit price -> total -> confidence`.

## Обязательные шаги

1. Определить тип PDF:
- `machine-readable text PDF`
- `scanned PDF`

2. Для text PDF:
- text extraction;
- table/line extraction;
- очистка шапки колонок от item names;
- нормализация числовых полей.

3. Для scanned PDF:
- rasterize;
- OCR;
- confidence;
- manual review на низкоуверенные строки.

4. Передать дальше:
- supplier cards;
- invoice ledger;
- price delta analysis;
- manual review queue.

## SOTA-подход для PDF

### Метод 1 — Cascade parsing
Не один extractor на всё, а каскад:
- text first;
- OCR fallback;
- evidence merge.

### Метод 2 — Header-noise cleaning
УПД и накладные часто прилипают к строкам товаров шапкой колонок.
Агент обязан чистить шум вида:
- `1а 1б 2 2а ...`
- коды колонок;
- служебные хвосты.

### Метод 3 — Dedup by source identity
Один и тот же PDF может лежать в `Новые документы`, `Обработано`, `Разное`.
Агент обязан убирать дубликаты до построения supplier turnover и price history.

### Метод 4 — Confidence split
Подтверждённые строки и спорные строки нельзя смешивать в одном слое решения.

## Quality gate

- [ ] Нет duplicate invoices в агрегатах.
- [ ] Нет header-noise в item names.
- [ ] Есть supplier/date/invoice extraction.
- [ ] Есть line items, а не только preview text.
