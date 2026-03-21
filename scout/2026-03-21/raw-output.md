Отлично, файлы сформированы. Теперь вывожу их содержимое в нужном формате для парсера.

=== FILE: morning-ideas.md ===
# Утренние идеи Scout (2026-03-21)

> ТОП-5 находок за ночь, ранжированные по приоритету.

---

## 1. Model Context Protocol (MCP) — Universal Standard (2026)

**Источник:** Anthropic Engineering Blog, LangChain Docs, Google Developers Blog  
**Релевантность:** критическая  
**Связь:** WP-73 (Архитектура платформы), WP-138 (Cloud runtime)

### Суть

MCP стал универсальным стандартом для подключения AI-агентов к enterprise-инструментам в 2026. Управляется Agentic AI Foundation под Linux Foundation. 97M+ ежемесячных скачиваний SDK, 75+ официальных коннекторов, принят Anthropic, OpenAI, Google, Microsoft.

### Reasoning

Прямая связь с WP-73 (новая архитектура платформы): MCP — это де-факто протокол интеграции агентов. Если IWE строит экосистему агентов, нужно проектировать вокруг MCP как стандарта, а не изобретать собственные протоколы. Это подтверждение правильности направления текущей архитектуры (MCP серверы уже используются).

### Предложение

- **Тип:** SOTA-обновление + архитектурная валидация
- **Куда:** PACK-digital-platform → DP.SOTA (новый раздел "Интеграция агентов")
- **Действие:** Capture: MCP как стандарт 2026 → DP.SOTA, валидировать архитектуру Phase 2

---

## 2. "Loopy Era" — Self-Improvement Loop как норма

**Источник:** Andrej Karpathy (NextBigFuture, март 2026)  
**Релевантность:** критическая  
**Связь:** WP-132 (автономные агенты), AS.M.001

### Суть

Карпати объявил наступление "loopy era" — эры агентов с непрерывными петлями самоулучшения на коде и исследованиях. AutoResearch запустил 700 экспериментов за 2 дня, открыл 20 оптимизаций, улучшивших обучение. Агенты редактируют train.py, пробуют идеи (включая новые архитектуры: переупорядочение QK Norm и RoPE), учатся на неудачах, продолжают без участия человека. Декабрь 2025 = порог когерентности для coding agents.

### Reasoning

Это подтверждение актуальности WP-132. "Loopy era" = то, что мы проектируем (improvement loop в AS.M.001). Но Карпати идёт дальше: агенты на уровне кода ML-экспериментов. Это новый масштаб автономии (T4 → T5?). Применимо к IWE: можно спроектировать self-improving pipeline для Pack-обновлений.

### Предложение

- **Тип:** SOTA-обновление + различение
- **Куда:** PACK-autonomous-agents → AS.SOTA + AS.D (Execution Loop ≠ Improvement Loop)
- **Действие:** Capture: Loopy Era (Karpathy, март 2026) → AS.SOTA. Различение: Execution Loop ≠ Improvement Loop

### Черновик различения

**Execution Loop ≠ Improvement Loop**

| Аспект | Execution Loop | Improvement Loop |
|--------|---------------|------------------|
| Цель | Решить задачу пользователя | Улучшить способность агента решать задачи |
| Артефакт | Рабочий продукт (код, текст, решение) | Обновлённый промпт, модель, процедура |
| Частота | Каждый запрос | После N запросов или по триггеру |
| Контекст | Входные данные задачи | Траектории успешных/неуспешных решений |
| Пример (IWE) | Scout находит идеи ночью | Scout читает feedback утром, обновляет правила |

---

## 3. Context Engineering: Write/Select/Compress/Isolate (WSCI)

**Источник:** LangChain Blog, Anthropic Engineering, Weaviate  
**Релевантность:** высокая  
**Связь:** WP-132 (trajectory cache), AS.SOTA.002

### Суть

Context Engineering детализируется через 4 стратегии: **Write** (формирование контекста), **Select** (выбор релевантного), **Compress** (сжатие для вместимости), **Isolate** (изоляция контекстов между агентами). Это не prompt engineering (выбор слов), а архитектурное проектирование контекстного окна как scarce resource. Агенты = пользователи и архитекторы контекста одновременно.

### Reasoning

Прямо применимо к WP-132: управление trajectory cache = комбинация Write (запись успешной траектории), Select (какие траектории брать как few-shot), Compress (сколько траекторий влезает в prompt), Isolate (контексты Scout и Extractor не пересекаются). AS.SOTA.002 уже содержит общий Context Engineering, но WSCI — конкретная детализация.

### Предложение

- **Тип:** SOTA-обновление (детализация)
- **Куда:** PACK-autonomous-agents → AS.SOTA.002 (расширить)
- **Действие:** Capture: WSCI детализация → AS.SOTA.002

---

## 4. WEF Trust Stack — 5 компонентов governance автономии

**Источник:** World Economic Forum, февраль 2026, Singapore IMDA  
**Релевантность:** высокая  
**Связь:** WP-132 (agent-card.yaml), AS.M

### Суть

WEF формализовал Trust Stack для автономных агентов с 5 компонентами: (1) Legible reasoning (объяснение, как пришли к решению), (2) Bounded agency (чёткие границы без эскалации), (3) Goal transparency (цели явно, не скрыты), (4) Contestability/override (человек может оспорить), (5) Governance by design (логирование, аудит встроены с нуля, не добавлены потом). Singapore IMDA выпустил Model AI Governance Framework for Agentic AI на основе Trust Stack.

### Reasoning

Уже частично в agent-card.yaml (WP-132), но WEF даёт авторитетную формализацию. Это не просто чеклист, а **методология проектирования** автономных агентов. Применимо как Pack-метод (AS.M): "Как спроектировать governance автономного агента".

### Предложение

- **Тип:** метод
- **Куда:** PACK-autonomous-agents → AS.M (новый: Trust Stack Design)
- **Действие:** Capture: WEF Trust Stack → AS.M

---

## 5. Meta REA: Real-world autonomous agent case (март 2026)

**Источник:** Engineering at Meta, 17 марта 2026  
**Релевантность:** средняя  
**Связь:** WP-132 (реальные кейсы), AS.USE-CASES

### Суть

Meta анонсировала Ranking Engineer Agent (REA) — автономный агент, выполняющий все шаги ML lifecycle для ads ranking моделей: генерация гипотез, запуск обучения, дебаг неудач, итерации на результатах. Реальный production use case автономного агента в enterprise.

### Reasoning

Запрос из fleeting-notes (строка 87-89): "Какие есть реальные кейсы работы автономных агентов?" — REA = answer. Это не академический пример, а production система Meta. Применимо как AS.USE-CASES (примеры реальных внедрений). Но для WP-132 менее критично (мы не делаем ML lifecycle agent).

### Предложение

- **Тип:** use case + SOTA-обновление
- **Куда:** PACK-autonomous-agents → AS.USE-CASES (новый раздел)
- **Действие:** Capture: Meta REA → AS.USE-CASES

---

=== FILE: capture-candidates.md ===
# Capture Candidates для Экстрактора (R2)

> Черновики для формализации в Pack. Экстрактор (R2) обрабатывает на Day Close.

---

## 1. Execution Loop ≠ Improvement Loop

**Тип:** различение  
**Pack:** PACK-autonomous-agents  
**Целевая сущность:** AS.D (новое различение)

### Черновик

#### X ≠ Y

**Execution Loop ≠ Improvement Loop**

#### Таблица сравнения

| Аспект | Execution Loop | Improvement Loop |
|--------|---------------|------------------|
| **Суть** | Агент решает задачу | Агент улучшает свою способность решать задачи |
| **Вопрос** | "Что делать с этим запросом?" | "Как стать лучше в решении таких запросов?" |
| **Артефакт** | Рабочий продукт (код, текст, решение) | Обновлённый промпт, модель, процедура |
| **Частота** | Каждый запрос | После N запросов или по триггеру |
| **Контекст** | Входные данные задачи | Траектории (успешные/неуспешные) |
| **Метрика** | Качество решения (accuracy, user satisfaction) | Прирост качества (Δ accuracy, Δ trajectory success rate) |
| **Пример (IWE)** | Scout находит идеи ночью | Scout читает feedback утром, обновляет правила |
| **Пример (Karpathy)** | AutoResearch запускает эксперимент | AutoResearch анализирует 700 результатов, улучшает train.py |

#### Источник

Andrej Karpathy, NextBigFuture, март 2026: "Loopy Era" — агенты с непрерывными improvement loops.

#### Failure mode

**Отсутствие Improvement Loop**: агент застревает на plateau производительности. Execution Loop работает, но качество не растёт. Пример: chatbot без feedback loop продолжает делать одни и те же ошибки.

---

## 2. WSCI Framework (Context Engineering детализация)

**Тип:** SOTA-обновление (детализация AS.SOTA.002)  
**Pack:** PACK-autonomous-agents  
**Целевая сущность:** AS.SOTA.002 (Context Engineering)

### Черновик

#### Обновление AS.SOTA.002

**Context Engineering (WSCI Framework, 2026)**

Context Engineering — проектирование контекстного окна LLM как scarce resource. Состоит из 4 стратегий:

1. **Write** — формирование контекста: что записать в memory, какие данные сохранить в trajectory cache
2. **Select** — выбор релевантного: какие траектории/факты брать как few-shot, какие документы из RAG
3. **Compress** — сжатие: сколько траекторий влезает в prompt, как суммаризировать длинный контекст
4. **Isolate** — изоляция контекстов: контексты разных агентов не пересекаются (SOTA.006 coordination cost)

**Отличие от Prompt Engineering**: Prompt Engineering = выбор правильных слов. Context Engineering = архитектурное проектирование вокруг ограниченного контекстного окна.

**Применение к IWE**:
- Write: Scout записывает успешные траектории в `trajectory-cache/`
- Select: Промпт Scout берёт ТОП-3 траектории как few-shot
- Compress: Промпт Scout ≤4K tokens (бюджет $1)
- Isolate: Scout и Extractor — разные контексты (SOTA.006)

#### Источники

- LangChain Blog: "Context Engineering for Agents" (2026)
- Anthropic Engineering: "Effective Context Engineering for AI Agents" (2026)
- Weaviate Blog: "Context Engineering - LLM Memory and Retrieval for AI Agents" (2026)

---

## 3. WEF Trust Stack

**Тип:** метод  
**Pack:** PACK-autonomous-agents  
**Целевая сущность:** AS.M (новый метод: Trust Stack Design)

### Черновик

#### AS.M.00X: Trust Stack Design (WEF, 2026)

**Назначение**: Методология проектирования governance автономных агентов.

**Контекст**: При проектировании автономного агента (autonomy_level ≥ execute) необходимо встроить governance с нуля, а не добавлять потом.

**Метод**: Trust Stack состоит из 5 компонентов (WEF, февраль 2026):

1. **Legible Reasoning** — агент объясняет, как пришёл к решению (на подходящем уровне детализации)
2. **Bounded Agency** — чёткие границы того, что агент может делать/решать/рекомендовать. Никакой скрытой эскалации полномочий
3. **Goal Transparency** — цели агента явные (accuracy, safety, efficiency, engagement, коммерческие KPI)
4. **Contestability & Override** — человек может оспорить, исправить или отключить агента
5. **Governance by Design** — логирование, аудит, oversight встроены с нуля (не добавлены потом)

**Применение к IWE** (agent-card.yaml):
- Legible Reasoning → Scout выводит reasoning для каждой находки
- Bounded Agency → `--allowedTools`, `--max-budget-usd`, lock-механизм
- Goal Transparency → `goal` в agent-card.yaml
- Contestability → результат в `night-results/`, человек ревьюит утром
- Governance by Design → логи в `logs/`, траектории в `trajectory-cache/`

#### Источники

- World Economic Forum: "How to design for trust in the age of AI agents" (февраль 2026)
- Singapore IMDA: "Model AI Governance Framework for Agentic AI" (2026)

---

## 4. MCP как стандарт 2026

**Тип:** SOTA-обновление  
**Pack:** PACK-digital-platform  
**Целевая сущность:** DP.SOTA (новый раздел "Интеграция агентов")

### Черновик

#### DP.SOTA.00X: MCP — Universal Standard for Agent Integration (2026)

**Статус**: Model Context Protocol (MCP) стал де-факто универсальным стандартом подключения AI-агентов к enterprise-инструментам в 2026.

**Факты**:
- Управляется Agentic AI Foundation под Linux Foundation
- 97M+ ежемесячных скачиваний SDK
- 75+ официальных коннекторов
- Принят Anthropic, OpenAI, Google, Microsoft

**Импликации для IWE**:
- Все интеграции агентов проектировать вокруг MCP (не изобретать собственные протоколы)
- MCP серверы — правильная архитектурная ставка (уже используются в IWE)
- При проектировании новой интеграции → проверить наличие MCP коннектора

**Связь с архитектурой**: WP-73 Phase 2 (спецификации модулей) → валидировать, что все агентные интеграции через MCP.

#### Источники

- Model Context Protocol (Anthropic, 2026)
- Google Developers Blog: "Architecting efficient context-aware multi-agent framework for production" (2026)

---

=== FILE: new-wp-proposals.md ===
# Предложения новых РП

---

## 1. Linear для организации агентских пайплайнов

**Источник:** fleeting-notes.md (строка 17-20)  
**Релевантность:** средняя  
**Связь:** WP-132 (автономные агенты), экосистема

### Суть идеи

"Рассматривать Linear для организации агентских пайплайнов? Аналогично работе команды живых людей."

### Reasoning

Сейчас агенты = bash scripts + Claude CLI. Linear = task tracker для команд. Идея: агенты как "сотрудники", задачи как Linear issues. Но нужно исследование: какие реальные кейсы Linear + AI agents? Какие инструменты есть? Оправдан ли overhead?

### Предложение РП

**Название**: Исследование: Linear для агентских пайплайнов  
**Приоритет**: низкий  
**Фаза**: Ф0 (исследование)  
**Критерии завершения**: Отчёт: (1) какие инструменты Linear + AI agents существуют, (2) реальные кейсы использования, (3) применимость к IWE, (4) рекомендация (да/нет/отложить).

---

## 2. SOTA методов написания постов (Scout для контента)

**Источник:** fleeting-notes.md (строка 46-49)  
**Релевантность:** средняя  
**Связь:** WP-119 (контент-конвейер)

### Суть идеи

"Разведчик для SOTA методов написания постов. Чтобы не было похоже на ИИ."

### Reasoning

Проблема: AI-generated content легко детектируется. Нужен Scout для методов написания, которые минимизируют "AI-похожесть". Связано с WP-119 (контент-конвейер). Можно спроектировать как расширение ночного Scout: добавить источник "writing techniques 2026", формировать рекомендации для улучшения черновиков.

### Предложение РП

**Название**: SOTA методов написания (антидетект AI)  
**Приоритет**: низкий  
**Фаза**: Ф0 (исследование), затем Ф1 (интеграция в Scout)  
**Критерии завершения**: (1) SOTA методов написания 2026, (2) чеклист антидетекта AI, (3) интеграция в Scout config (новый источник), (4) применено к 3 черновикам из WP-119.

---

=== END ===
