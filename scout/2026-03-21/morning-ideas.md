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

