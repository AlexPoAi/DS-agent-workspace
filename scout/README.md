# Scout (R23) — review/input слой

## Роль

`Scout` не является вторым `Strategist` и не является параллельным центром приоритетов.

Его задача:

- сканировать источники и находить сигналы;
- формировать находки, capture-кандидаты и WP-кандидаты;
- отдавать это в ядро Церена для review;
- не строить собственную competing-доску и не подменять `DayPlan`/`WeekPlan`.

Целевой маршрут:

`Scout -> report/candidates -> Strategist review -> DayPlan / WeekPlan / Требует внимания`

## Входы

- `backlog.yaml` — задания типа `scan`
- `trajectory/rules.md` — накопленные правила quality/relevance
- `trajectory/accepted.md` / `rejected.md` — feedback loop
- активный контекст дня и недели из `DS-strategy/current/DayPlan*.md` и `WeekPlan*.md`

## Выходы

Писать только в `scout/results/YYYY/MM/DD/`:

- `report.md` — единый отчёт для review
- `morning-ideas.md` — raw findings
- `capture-candidates.md` — кандидаты для `Extractor`
- `new-wp-proposals.md` — кандидаты новых WP
- `meta.yaml` — служебные метаданные

## Что Scout НЕ делает

- не меняет `DayPlan`, `WeekPlan`, `WP-REGISTRY` напрямую;
- не пишет в `MEMORY.md`;
- не создаёт вторую `Доску выбора`;
- не принимает решения о приоритетах сам.

## Как использовать

1. Запустить `Scout` в отдельной bounded session.
2. Получить output только в `scout/results/...`.
3. Провести review через `Strategist` или вручную.
4. Только после review переносить:
   - знания -> `captures.md` / Pack,
   - новые work products -> `DS-strategy/inbox/`,
   - attention items -> `DayPlan` / `WeekPlan`.

## Truth note

На `2026-04-29` `Scout` в этом репозитории — это рабочий data/review layer, но не подтверждённый обязательный live runtime service.
