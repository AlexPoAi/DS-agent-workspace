---
skill: warehouse-abc-intake-and-matching
agent: warehouse-demand-analyst
created: 2026-04-21
status: active
---

# Warehouse ABC Intake And Matching

## Цель

Делать из weekly `ABC` не артефакт “для галочки”, а реальный сигнал
приоритета закупки и анти-перезатарки.

## Общий discipline gate

Применять `agency/skills/common/codex-karpathy-discipline.md`.

Для ABC это значит:
- не угадывать класс SKU при слабом match;
- не создавать новые alias-правила без evidence;
- не менять target stock только потому, что позиция кажется важной;
- проверять matched/unmatched и выносить спорное в manual review.

## Обязательные шаги

1. Найти актуальный `ABC`-источник:
- какой файл свежий;
- какой лист содержит таблицу;
- где summary block, а где строки позиций.

2. Извлечь:
- `SKU / наименование`;
- `ABC class`;
- при наличии `qty`, `revenue`, `share`, `margin`.

3. Нормализовать названия:
- привести к тем же именам, что в остатках/продажах;
- схлопнуть регистр, пробелы, типовые aliases;
- отделить категорийные строки (`НАПИТКИ`, `Кофе`) от SKU-строк.

4. Сопоставить с warehouse universe:
- matched SKU;
- unmatched SKU;
- ambiguous matches.

5. Поднять в decision layer только подтверждённые категории.

## SOTA-подход для ABC

### Метод 1 — Hierarchy-aware parsing
Уметь различать:
- total row;
- category rollup row;
- actual SKU row.

### Метод 2 — Semantic header mapping
Колонка `ABC` может быть не названа прямо.
Агент должен уметь находить её по паттерну значений `A/B/C`,
если имя колонки нестандартное.

### Метод 3 — Match ledger
Всегда сохранять:
- сколько matched;
- сколько unmatched;
- preview проблемных строк.

### Метод 4 — Decision-safe fallback
Если часть low-stock позиций без `ABC`,
агент не выдумывает класс, а помечает:
- `решение пока по остатку`;
- `ABC category not matched`.

## Quality gate

- [ ] Таблица `ABC` действительно прочитана до конца.
- [ ] SKU-строки отделены от category rollups.
- [ ] `matched/unmatched` зафиксированы.
- [ ] `ABC` реально влияет на reorder-priority.
