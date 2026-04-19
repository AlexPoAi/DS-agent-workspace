---
skill_id: knowledge-registry-curator.notes-registry-and-domain-mapping
agent: knowledge-registry-curator
status: active
version: 1.0
created: 2026-04-19
updated: 2026-04-19
migration_target: DS-strategy/inbox/WP-82-agent-skills-pack-architecture (Отдельный PACK для скиллов агентов).md
---

# Skill: Notes Registry and Domain Mapping

## Зачем нужен

Этот skill превращает сырой слой заметок и backlog в управляемую карту знаний для стратегического слоя.

Skill нужен, чтобы `Knowledge Registry Curator` мог одинаково работать с:
- extractor outputs;
- weekly note batches;
- backlog slices;
- доменной картой и Pack coverage.

## Когда запускать

- Когда нужно собрать или обновить `Notes Registry`
- Когда нужно разложить backlog/notes по доменам
- Когда нужно найти `orphan notes`
- Когда нужно увидеть `missing domains`
- Когда `Strategist` просит подготовить чистый handoff для weekly portfolio

## Не запускать когда

- Нужно чинить технический ingestion/extractor pipeline
- Нужно принимать weekly priorities или выбирать главный WP недели
- Нужно выполнять доменную задачу, а не классифицировать knowledge-layer

## Входы

Минимальный входной пакет:
- `current/SESSION-CONTEXT.md`
- `inbox/INBOX-TASKS.md`
- extractor outputs / extraction reports
- `UNPROCESSED-NOTES-REPORT.md` если он есть
- активные `WP-*`, связанные с backlog, notes layer и taxonomy

Дополнительный входной пакет:
- `WeekPlan`
- Pack-реестры и `CONTEXT.md` нужных доменов
- прошлые карты backlog/domains, если уже есть

## Выходы

Skill должен уметь производить 4 типа результата:

1. `Notes Registry Update`
2. `Domain Map`
3. `Exceptions Report`
4. `Strategist Handoff`

## Обязательные поля реестра

Для каждой записи определить:
- `note_id`
- `source`
- `status`
- `note_type`
- `domain`
- `subdomain`
- `pack`
- `wp_link`
- `priority`
- `next_action`

Если поле нельзя определить уверенно:
- не выдумывать;
- ставить `unknown` или `needs-review`;
- выносить запись в doubtful classifications.

## Режимы работы

### 1. ingest week notes

Цель:
- собрать недельный пакет заметок в единый реестр.

Что сделать:
- принять batch заметок недели;
- проставить статусы и типы;
- связать заметки с доменами и Pack;
- вынести orphan notes.

### 2. classify backlog slice

Цель:
- разложить отдельный backlog-slice по предметным областям.

Что сделать:
- пройти записи по одной;
- отделить заметку от задачи, WP и carry-over;
- назначить домен и поддомен;
- проверить, есть ли уже активный WP по домену.

### 3. find missing domains

Цель:
- понять, каких предметных областей или Pack-слоёв не хватает.

Что сделать:
- проверить кластеры заметок без устойчивого домена;
- найти повторяющиеся темы без Pack/registry слоя;
- сформировать список missing domains.

### 4. prepare strategist handoff

Цель:
- отдать Стратегу чистую картину для weekly decisions.

Что сделать:
- собрать домены;
- показать один главный активный WP на домен;
- показать очередь домена;
- вынести exceptions и risky classifications.

## Пошаговый workflow

### Шаг 1 — Собери сырьё

Прочитай notes/backlog/context слой и проверь, что scope понятен:
- batch недели;
- конкретный backlog slice;
- или полный registry update.

### Шаг 2 — Нормализуй записи

Для каждой единицы различи:
- заметка;
- backlog item;
- WP;
- доменная сущность;
- Pack reference.

### Шаг 3 — Назначь домен и Pack

Каждой записи назначь:
- domain
- subdomain
- pack
- wp link

Если уверенности нет, не форсируй классификацию.

### Шаг 4 — Собери карту

Покажи:
- сколько записей в домене;
- какой WP уже активен;
- что остаётся в очереди;
- где есть пустоты.

### Шаг 5 — Подготовь handoff

Собери короткий strategist-facing результат:
- что вошло в реестр;
- что осталось orphan;
- какие missing domains появились;
- какой один WP на домен виден сейчас.

## Quality gates

- Не смешивать `note`, `task`, `WP`, `Pack`
- Не придумывать домен без достаточного сигнала
- Missing domains фиксировать явно
- Если есть конфликт между несколькими доменами, пометить `needs-review`
- Итог должен уменьшать ручную работу `Strategist`, а не переносить туда хаос

## Шаблон результата

```markdown
# Notes Registry Update

## Registry Slice
- [note_id] source / status / note_type / domain / pack / next_action

## Domain Map
- [domain] active_wp / queue / coverage / gaps

## Exceptions
- orphan notes
- missing domains
- doubtful classifications

## Strategist Handoff
- [domain] one active WP
- [domain] next move

---
Skill: notes-registry-and-domain-mapping
Agent: Knowledge Registry Curator
Date: [YYYY-MM-DD]
Status: [draft / ready / review]
```

## Связь с контуром

- Upstream: `Extractor`
- Main consumer: `Knowledge Registry Curator`
- Downstream owner: `Strategist`
- Future migration: `PACK-agent-skills` (`WP-82`)
