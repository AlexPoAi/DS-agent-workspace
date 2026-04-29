---
type: agency-registry
version: 1.3
updated: 2026-04-29
---

# Реестр ИИ-агентов DS-агентства

> **Что это:** Каждый агент = специализированная роль поверх модели Claude.
> Модель (Sonnet/Haiku/Opus) — мощность. Агент — специализация + контекст домена + инструменты + контракт.
>
> **Когда нанимать:** после выбора модели в Ритуале согласования, если задача требует
> глубокого знания конкретного домена или устойчивой роли.

---

## Как читать карточку

| Поле | Значение |
|------|---------|
| **Домен** | Какой Pack / область знаний |
| **Специализация** | Что умеет лучше обычного Claude |
| **Модель по умолчанию** | Рекомендуемая модель для этой роли |
| **Инструменты** | Что использует (RAG, API, скрипты) |
| **Нанимать когда** | Конкретные триггеры |
| **Не нанимать когда** | Граница компетенции |

## Общий слой дисциплины

Все агенты агентства применяют `agency/skills/common/codex-karpathy-discipline.md` как лёгкий рабочий gate:
- сначала явные допущения и критерий успеха;
- затем минимальный scope без speculative work;
- работа коротким циклом `plan -> act -> verify -> adjust`;
- остановка и пересогласование, если scope, права доступа или риск для чужих dirty changes изменились.

---

## 1. VK Coffee Analyst (Аналитик кофейни)

| | |
|-|-|
| **Домен** | VK-offee Pack (BAR, KITCHEN, SERVICE, HR, MANAGEMENT) |
| **Специализация** | Меню, цены, рецепты, калькуляции, стандарты обслуживания, воронка продаж |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | RAG-бот VK-offee, PACK-bar, PACK-kitchen, Saby данные |
| **Нанимать когда** | Работа с рецептурой, стандартами, прайсами, анализ смен |
| **Не нанимать когда** | Архитектурные решения, работа с парком, юридика |
| **Файл** | `agency/agents/vk-coffee-analyst.md` ✅ |

---

## 2. Park Architect (Архитектор парка)

| | |
|-|-|
| **Домен** | VK-offee PACK-park-development (Парк Голубинка) |
| **Специализация** | Документы ГПЗУ, договоры, переписка с архитекторами и РНС, строительный процесс |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | PACK-park-development, карточки документов (PARK.WP.*, PARK.DOC.*) |
| **Нанимать когда** | Письма архитекторам, анализ документов, подготовка к РНС |
| **Не нанимать когда** | Вопросы кофейни, HR, технические системы |
| **Файл** | `agency/agents/park-architect.md` |

---

## 3. HR Specialist (HR-специалист)

| | |
|-|-|
| **Домен** | VK-offee PACK-hr (договоры, персонал, реквизиты) |
| **Специализация** | ГПХ, реквизиты ИП Жуковой, трудовые отношения, регламенты персонала |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | PACK-hr, archive/Персонал, HR.WP.001-004 |
| **Нанимать когда** | Работа с договорами, реквизитами, кадровые вопросы |
| **Не нанимать когда** | Техника, архитектура, стратегия |
| **Файл** | `agency/agents/hr-specialist.md` |

---

## 4. Environment Engineer (Инженер среды)

| | |
|-|-|
| **Домен** | FMT-exocortex-template, DS-strategy, close-task.sh, агенты launchd |
| **Специализация** | Скрипты агентов, диагностика сбоев, исправление среды, обновления от Церена |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | Bash, git, launchd logs, validate-template.sh, daily-report.sh |
| **Нанимать когда** | Сбои агентов, правки скриптов, обнови-экзокортекс, диагностика |
| **Не нанимать когда** | Доменные задачи VK-offee, стратегические решения |
| **Файл** | `agency/agents/environment-engineer.md` |

---

## 5. Strategist (Стратег)

| | |
|-|-|
| **Домен** | DS-strategy (планы, РП, WeekPlan, стратегические решения) |
| **Специализация** | Формирование РП, анализ INBOX-TASKS, планирование недели, StrategyReport |
| **Модель по умолчанию** | Opus |
| **Инструменты** | DS-strategy/current/, INBOX-TASKS.md, MEMORY.md |
| **Нанимать когда** | Сессии стратегирования, планирование недели, разбор inbox |
| **Не нанимать когда** | Реализационные задачи (нужны профильные агенты) |
| **Файл** | `agency/agents/strategist.md` |

---

## 6. Code Engineer (Инженер кода)

| | |
|-|-|
| **Домен** | DS-instrument репо (боты, MCP, API, Telegram-бот VK-offee) |
| **Специализация** | Python, bash, FastAPI, Telegram bot API, RAG pipeline, MCP серверы |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | VK-offee-rag, telegram-bot/, extractor.sh, Glob/Grep |
| **Нанимать когда** | Разработка ботов, API интеграции, скрипты, MCP |
| **Не нанимать когда** | Доменные знания кофейни, стратегия, юридика |
| **Файл** | `agency/agents/code-engineer.md` ✅ |

---

## 7. FPF Консультант

| | |
|-|-|
| **Домен** | FPF / SPF / domain-modeling / Pack VK-offee |
| **Специализация** | Domain-contract моделирование, FPF-инспекция типологий, принципы A.3/A.7/A.12/A.17, различения |
| **Модель по умолчанию** | gpt-4o (OpenAI Assistants API) |
| **Инструменты** | `fpf-consult.sh`, vector store: FPF-Spec.md (43k строк) + DOMAIN-CONTRACTS-ALL |
| **Нанимать когда** | Создание/проверка domain-contract карточек, FPF-инспекция, «это объект или процесс?», «это тип или экземпляр?» |
| **Не нанимать когда** | Реализационные задачи кофейни, код, стратегия — он только моделирует |
| **Файл** | `agency/agents/fpf-consultant.md` ✅ |

---

## 8. Supreme HR (Верховный HR-агент)

| | |
|-|-|
| **Домен** | agency/agents/, REGISTRY-EXTENDED.md, GAPS.md |
| **Специализация** | Подбор агентов под задачу из реестра 185+, фиксация пробелов |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | hire.sh, REGISTRY-EXTENDED.md, GAPS.md |
| **Нанимать когда** | «Кто из агентства может...», в Ритуале согласования (шаг «выбор агента») |
| **Не нанимать когда** | Когда агент уже выбран и нужна реализация |
| **Файл** | `agency/agents/supreme-hr.md` ✅ |

---

## 9. Warehouse Demand Analyst (Аналитик склада и спроса)

| | |
|-|-|
| **Домен** | VK-offee `PACK-warehouse` + `knowledge-base/Отчёты для бота` |
| **Специализация** | Остатки, продажи, ABC-приоритизация, рекомендации по закупке/перезатарке |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | `warehouse_reports_pipeline.py`, `warehouse_full_loop.sh`, `WH.REGISTRY`, складские карточки |
| **Нанимать когда** | Нужно быстро разобрать новые файлы Жанны и получить actionable план закупки |
| **Не нанимать когда** | Нужен ремонт инфраструктуры или стратегический план без данных склада |
| **Файл** | `agency/agents/warehouse-demand-analyst.md` ✅ |

---

## 10. Knowledge Registry Curator

| | |
|-|-|
| **Домен** | `DS-strategy`, extractor outputs, notes registry, domain mapping |
| **Специализация** | Реестр заметок, доменная карта, pack coverage, orphan notes, handoff в стратегический слой; русское описание роли — `Библиотекарь` |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | extractor reports, `UNPROCESSED-NOTES-REPORT`, `INBOX-TASKS`, `WeekPlan`, Pack-реестры |
| **Нанимать когда** | Нужно разложить заметки по доменам, собрать `Notes Registry`, показать missing domains и подготовить strategist handoff |
| **Не нанимать когда** | Нужны weekly-решения, инженерная реализация Extractor или доменная реализация конкретного WP |
| **Файл** | `agency/agents/knowledge-registry-curator.md` ✅ |

---

## 11. Park Permitting & Infrastructure Coordinator

| | |
|-|-|
| **Домен** | `VK-offee/PACK-park-development`, внешние согласования, инфраструктурные запросы, официальный документный цикл |
| **Специализация** | Официальные письма и запросы, реквизиты отправителя, clean-docs, сохранение в Drive, tracking входящих и follow-up |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | `PACK-park-development`, Google Drive, `COMM/DOC/WP` карточки, реестры проекта |
| **Нанимать когда** | Нужно подготовить официальный запрос, довести письмо до `ready-to-send`, сохранить документ и зафиксировать follow-up |
| **Не нанимать когда** | Нужен анализ проектной документации, архитектурное решение или общая weekly-стратегия |
| **Файл** | `agency/agents/park-permitting-infrastructure-coordinator.md` ✅ |

---

## 12. Financial Consultant

| | |
|-|-|
| **Домен** | `DS-finance` — cash discipline, finance views, project funding, межконтурные финансовые verdict |
| **Специализация** | `freeze / keep / invest`, funding gap, различение `opex / capex`, financial view по `VK Coffee`, `Park` и следующим доменам |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | `DS-strategy/current`, finance-related `WP`, domain source docs, будущие `Finance View` |
| **Нанимать когда** | Нужно понять, что можно тратить, что заморозить, сколько не хватает на проект и какой cash verdict честный |
| **Не нанимать когда** | Нужна бухгалтерская проводка, инженерный runtime fix или чистая weekly orchestration без финансового вопроса |
| **Файл** | `agency/agents/financial-consultant.md` ✅ |

---

## 13. VK Finance Analyst (Аналитик финансовых данных)

| | |
|-|-|
| **Домен** | `DS-finance-private` / VK-offee / Google Drive (Саби-отчёты) |
| **Специализация** | Ingestion Excel-отчётов из Drive, нормализация по точкам, Finance View (выручка, маржа, тренд, аномалии) |
| **Модель по умолчанию** | Sonnet |
| **Инструменты** | `google_drive_parser.py`, Drive папки Продажи/Выручка, `DS-finance-private/business-finance/` |
| **Нанимать когда** | Жанна загрузила новые отчёты Сабы → нужен Finance View; инвентаризация финансов точек |
| **Не нанимать когда** | Нужен verdict freeze/keep/invest (→ Financial Consultant); нужен анализ остатков/закупок (→ Warehouse Demand Analyst) |
| **Файл** | `agency/agents/vk-finance-analyst.md` ✅ |

---

## Матрица выбора

| Задача | Агент | Модель |
|--------|-------|--------|
| Подобрать агента под задачу | Supreme HR | Sonnet |
| Написать рецептуру / стандарт | VK Coffee Analyst | Sonnet |
| Разобрать свежий склад + ABC + закупки | Warehouse Demand Analyst | Sonnet |
| Собрать реестр заметок / карту доменов | Knowledge Registry Curator | Sonnet |
| Подготовить официальный запрос / follow-up по Парку | Park Permitting & Infrastructure Coordinator | Sonnet |
| Инвентаризация финансов / Finance View по точкам | VK Finance Analyst | Sonnet |
| Собрать finance view / funding gap / cash verdict | Financial Consultant | Sonnet |
| Письмо архитектору / анализ парка | Park Architect | Sonnet |
| Исправить ГПХ / кадровый вопрос | HR Specialist | Sonnet |
| Починить агента / среду | Environment Engineer | Sonnet |
| Планирование недели / РП | Strategist | Opus |
| Написать бота / API / скрипт | Code Engineer | Sonnet |
| Domain-contract / FPF-инспекция | FPF Консультант | gpt-4o |
| Быстрый поиск / простой вопрос | — (без агента) | Haiku |

---

## Как нанять в Ритуале согласования

```
Шаг 1: Выбор модели     → Sonnet / Haiku / Opus
Шаг 2: Выбор агента     → [имя] из реестра, или «без агента»

Пример:
  Модель: Sonnet
  Агент: VK Coffee Analyst
  Задача: разработать стандарт продажи маркета
```

Агент активируется загрузкой своей карточки (`agents/{slug}.md`) в контекст перед началом работы.

---

*Реестр обновляется при добавлении нового агента. Каждый агент = отдельный файл в `agency/agents/`.*
