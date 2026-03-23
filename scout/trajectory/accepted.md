# Принятые находки Scout (few-shot примеры)

> Этот файл обновляется автоматически через S54 (Feedback Loop).
> Scout использует его как few-shot для калибровки качества.

## 2026-03-20 #1: Автономия ≠ Автоматизация (WEF)

**Источник:** WEF «AI Agents in Action»
**Тип:** различение
**Куда:** PACK-autonomous-agents → AS.D.001

### Суть
WEF определяет автономию как «гибкость принятия решений» (агент выбирает что делать), а автоматизацию как «надёжное исполнение» (система делает что запрограммировано). Это разные классы систем с разными требованиями к governance.

### Reasoning
Прямо применимо к WP-132: scheduler.sh = автоматизация, новые агенты = автономия. Объясняет почему нужен Trust Stack, а не просто мониторинг. Содержит чёткое различение X ≠ Y.

---

## 2026-03-20 #2: WISC Framework (Context Engineering)

**Источник:** LangChain Blog, Victor Dibia Newsletter
**Тип:** SOTA-обновление
**Куда:** PACK-digital-platform → DP.SOTA.002 (расширение)

### Суть
Context Engineering детализируется в WISC: Write (формирование контекста), Isolate (изоляция контекстов между агентами), Select (выбор релевантного), Compress (сжатие для вместимости). Конкретнее чем общий SOTA.002.

### Reasoning
Прямо применимо к управлению trajectory cache в Scout и к проектированию multi-agent pipeline.

### Правка (2026-03-23)
Добавлена детализация по Anthropic Cookbook: конкретный маппинг на IWE-агентов (Scout→Write, scheduler→Isolate, dispatcher→Select, trajectory-cache→Compress). Ценное: переход от абстрактного фреймворка к конкретным ролям. Подтверждает: WISC = платформенный паттерн (DP.SOTA), не метод агента (AS.M).

---

## 2026-03-20 #3: GEPA Framework (Self-Improvement через Trajectory)

**Источник:** OpenAI Cookbook, Awesome-Self-Evolving-Agents
**Тип:** метод
**Куда:** PACK-autonomous-agents → AS.M.001 (расширение)

### Суть
Конкретный алгоритм self-improvement через trajectory: Generate → Evaluate → Prune → Augment.

### Reasoning
Прямая реализация для AS.M.001 (improvement loop design). Ценное: пошаговая процедура, не просто принцип.

### Правка (2026-03-23)
Обогащена конкретикой из ICLR 2026 Oral: (1) метрики GEPA vs GRPO/MIPROv2 (+40% task success, -30% hallucination), (2) failure modes FM.001 (Overfitting to positive trajectories) + FM.002 (Evaluation Function Misalignment), (3) mitigation через diverse seed examples + proxy metrics. Подтверждает: GEPA не просто «идея», а метод с измеримыми результатами. AS.M.001 существует → расширить его этими данными, не создавать AS.M.002.

---

## 2026-03-23 #4: Production Autonomous Agents — ROI и Operationalization Gap

**Источник:** McKinsey Digital, Singapore IMDA
**Тип:** SOTA-обновление + Failure Mode
**Куда:** (1) PACK-autonomous-agents → AS.SOTA.002, (2) AS.FM.008

### Суть
Конкретные ROI benchmarks: 200–500% ROI в customer service (McKinsey), 30–60% reduction in manual review (Singapore IMDA). Критический failure mode: 40% проектов провалились из-за организационных причин (Operationalization Gap), не технических. Governance + change management = необходимость, не опция.

### Reasoning
Прямая релевантность для WP-142 (fundraising pitch). Отвечает на вопрос «а сколько это стоит и что даёт?». AS.FM.008 объясняет, почему хорошая технология может провалиться. Ценное: количественные данные для стратегических решений.

---

## 2026-03-23 #5: WEF Trust Stack — 5-Layer Model + Progressive Governance

**Источник:** WEF «AI Agents in Action», Singapore IMDA
**Тип:** метод
**Куда:** PACK-autonomous-agents → AS.M.001 (расширение Trust Stack Design)

### Суть
Детализация Trust Stack в 5 слоёв: (1) Legible Reasoning, (2) Bounded Agency, (3) Goal Transparency, (4) Contestability, (5) Oversight. Progressive Governance: T0 (no autonomy) → T4 (full autonomy), safeguards пропорционально уровню.

### Reasoning
AS.M.001 уже содержит Trust Stack Design. Это расширение с конкретной структурой и континуумом governance. Ценное: переход от чеклиста к проектируемой системе. Правило: не создавать AS.M.003, расширить AS.M.001.

