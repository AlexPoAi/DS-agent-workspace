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

