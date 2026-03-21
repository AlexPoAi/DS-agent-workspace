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
│   └── YYYY-MM-DD/
│       ├── meta.yaml
│       ├── morning-ideas.md
│       ├── capture-candidates.md
│       └── new-wp-proposals.md
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
│   └── YYYY-MM-DD/
│       └── scheduler-report.md
└── {new-agent}/                   ← будущие агенты
    └── YYYY-MM-DD/
```

---

## 4. Конвенции

### 4.1. Размещение

Каждый агент пишет в `{agent-name}/YYYY-MM-DD/`. Формат выхода определяется agent-card в DS-autonomous-agents.

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

*Создан: 2026-03-21*
