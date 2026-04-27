# DS-agent-workspace — инструкции для Claude

> **Общие инструкции:** см. `/Users/tserentserenov/IWE/CLAUDE.md`
>
> Этот файл содержит только специфику данного репозитория.

---

## 1. Тип репозитория

**DS/governance** — шина данных автономных агентов IWE.

**Source-of-truth:** нет. Это производные данные, не первоисточник.

---

## 2. Назначение

Хранит **выходные артефакты** всех автономных агентов платформы:
- Черновики (DayPlan-draft, WeekPlan-draft)
- Отчёты (verify-report, extraction-report)
- Находки (morning-ideas, capture-candidates, new-wp-proposals)
- Результаты ночных циклов (scheduler-reports)

**Различение:** Код агентов → DS-autonomous-agents (instrument). Данные агентов → здесь (governance). Утверждённые решения → DS-my-strategy (governance).

**Важно:** наличие папки агента в этом репозитории не означает, что для неё существует отдельный живой `launchd`/runtime сервис. Здесь смешаны:
- live runtime core (`strategist`, `extractor`, `scheduler`);
- artifact-bus для результатов;
- research/trajectory слои (`scout`);
- заготовки под будущие роли (`verifier`, `{new-agent}`).

---

## 3. Структура

```
DS-agent-workspace/
├── CLAUDE.md                      ← этот файл
├── REPO-TYPE.md
├── scout/                         ← Research/trajectory слой, не обязательный live runtime
│   ├── backlog.yaml               ← бэклог заданий Разведчику
│   ├── results/
│   │   └── YYYY/MM/DD/
│   │       ├── report.md          ← ЕДИНЫЙ отчёт (для ревью)
│   │       ├── raw-output.md      ← сырой вывод Claude
│   │       ├── morning-ideas.md   ← находки (raw)
│   │       ├── capture-candidates.md
│   │       ├── new-wp-proposals.md
│   │       └── meta.yaml
│   └── trajectory/                ← обратная связь
│       ├── rules.md
│       ├── accepted.md
│       ├── rejected.md
│       └── archive/
├── verifier/                      ← Заготовка/placeholder роли, не подтверждённый runtime
│   └── README.md                  ← truth-note о placeholder-статусе
├── strategist/                    ← Live core role (R1), сценарии обычно запускает scheduler
│   └── .gitkeep                   ← runtime truth смотрим в status/log layer, не здесь
├── extractor/                     ← Live core role (R2)
│   └── extraction-reports/        ← реальные extraction reports текущего контура
│       └── YYYY-MM-DD-inbox-check.md
├── scheduler/                     ← Live core control plane (cron/launchd)
│   ├── reports/
│   │   ├── SchedulerReport YYYY-MM-DD.md
│   │   └── archive/              ← старые отчёты
│   ├── scheduler-reports/         ← legacy historical path до выравнивания структуры
│   ├── feedback-triage/
│   │   └── YYYY-MM-DD.md         ← QA бота (feedback_triage DB)
│   ├── day-close.log
│   └── open-sessions.log
└── {new-agent}/                   ← будущие агенты
    └── ...
```

---

## 4. Конвенции

### 4.1. Размещение

Каждый агент пишет в свою папку, но исторически структура получилась не полностью унифицированной:
- Scout: `scout/results/YYYY/MM/DD/` (иерархия год/месяц/день) + `scout/backlog.yaml` + `scout/trajectory/`
- Extractor: `extractor/extraction-reports/YYYY-MM-DD-inbox-check.md`
- Scheduler: текущий truth-path `scheduler/reports/`, плюс legacy-хвост `scheduler/scheduler-reports/`
- Strategist: runtime результаты не считаем канонически живущими в этой папке; truth смотрим через status/log layer и `DS-strategy/current/*`

Формат выхода определяется agent-card в DS-autonomous-agents.

### 4.1.1. Legacy vs current

- `scheduler/reports/` — текущий канонический путь для свежих scheduler reports.
- `scheduler/scheduler-reports/` — исторический архивный хвост; не считать его live source-of-truth.
- `extractor/extraction-reports/` — текущий канонический путь для extraction reports в этом repo.
- `strategist/` и `verifier/` сейчас не являются надёжным местом для оценки live активности роли.

### 4.2. Truth for runtime

Если нужно понять, что реально живо сейчас, смотреть нужно не на список папок, а на:
- `launchctl`
- `~/.local/state/exocortex/status/*`
- `DS-strategy/current/AGENTS-STATUS.md`

### 4.3. Именование папок

Имя папки = имя роли (lowercase, kebab-case). Примеры: `scout`, `verifier`, `strategist`, `extractor`, `night-wp-runner`.

### 4.4. Retention

- Результаты хранятся 30 дней
- Архивация при Week Close (перемещение в `{agent}/archive/`)
- meta.yaml сохраняется для статистики

### 4.5. Pipeline raw → approved

```
Агент создаёт артефакт
  → DS-agent-workspace/{agent}/YYYY-MM-DD/
  → Человек ревьюит
  → Утверждённая версия → DS-my-strategy (или Pack)
  → Черновик остаётся в workspace (аудит, обучение)
```

---

## 5. Связи

| Репозиторий | Роль |
|-------------|------|
| DS-autonomous-agents | Код агентов (instrument) — производители |
| DS-my-strategy | Утверждённые решения (governance) — потребитель |
| PACK-* | Формализованные знания — конечный потребитель через Экстрактора |

---

*Создан: 2026-03-21. Обновлён: 2026-03-25 (WP-176: scheduler reports + feedback-triage)*
