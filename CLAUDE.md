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

---

## 3. Структура

```
DS-agent-workspace/
├── CLAUDE.md                      ← этот файл
├── REPO-TYPE.md
├── scout/                         ← Разведчик (R23)
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
├── verifier/                      ← Верификатор (VR.R.001)
│   └── YYYY-MM-DD/
│       └── verify-report.md
├── strategist/                    ← Стратег (R1)
│   └── YYYY-MM-DD/
│       └── dayplan-draft.md
├── extractor/                     ← Экстрактор (R2)
│   └── YYYY-MM-DD/
│       └── extraction-report.md
├── scheduler/                     ← Планировщик (cron/launchd)
│   ├── reports/
│   │   ├── SchedulerReport YYYY-MM-DD.md
│   │   └── archive/              ← старые отчёты
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

Каждый агент пишет в свою папку. Структура внутри — по усмотрению агента:
- Scout: `scout/results/YYYY/MM/DD/` (иерархия год/месяц/день) + `scout/backlog.yaml` + `scout/trajectory/`
- Остальные: `{agent-name}/YYYY-MM-DD/` (плоская структура по дате)

Формат выхода определяется agent-card в DS-autonomous-agents.

### 4.2. Именование папок

Имя папки = имя роли (lowercase, kebab-case). Примеры: `scout`, `verifier`, `strategist`, `extractor`, `night-wp-runner`.

### 4.3. Retention

- Результаты хранятся 30 дней
- Архивация при Week Close (перемещение в `{agent}/archive/`)
- meta.yaml сохраняется для статистики

### 4.4. Pipeline raw → approved

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
