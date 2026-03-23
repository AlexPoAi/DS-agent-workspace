# Scout Morning Ideas — 2026-03-23

> Топ-5 находок, ранжированные по приоритету для текущих РП и стратегических фокусов.

---

## 1. GEPA Framework — Self-Improvement через Trajectory Evolution

**Источник:** [GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning](https://arxiv.org/abs/2507.19457) (arXiv, принят на ICLR 2026 Oral)

**Релевантность:** КРИТИЧЕСКАЯ  
**Связь:** WP-144 (Автономные агенты Ф7 — саморазвитие), PACK-autonomous-agents AS.M.001

### Суть
GEPA (Genetic-Pareto) — конкретный алгоритм self-improvement через trajectory caching + reflexion. В отличие от RL, который схлопывает execution trace в scalar reward, GEPA читает ПОЛНЫЕ траектории (error messages, reasoning logs, tool outputs) и рефлексирует на естественном языке: диагностирует проблему → предлагает fix → тестирует → комбинирует уроки с Pareto frontier. 

**Performance (2026):** Обгоняет GRPO на 6% в среднем и до 20% на некоторых задачах, используя в 35x меньше rollouts. Обгоняет MIPROv2 на 10%+ (например, +12% accuracy на AIME-2025).

### Reasoning
Прямой ответ на WP-144 Ф7 (self-improvement loop). Сейчас в Pack есть AS.M.001 (improvement loop design), но он абстрактный. GEPA — конкретная реализация с проверенными метриками ROI. Это НЕ просто SOTA-обновление — это **метод** с пошаговым алгоритмом: Generate → Evaluate → Prune → Augment, применимый к Scout/Strategist/любому агенту.

Также решает проблему из fleeting-notes: "симбиоз автономных агентов" — GEPA позволяет агентам учиться на своих траекториях БЕЗ human-in-the-loop.

### Предложение
- **Тип:** метод (пошаговый алгоритм)
- **Куда:** PACK-autonomous-agents → AS.M.001 (расширение) или новый AS.M.002-GEPA
- **Действие:** Capture: GEPA Framework → AS.M.001/002

### Черновик для Экстрактора

```markdown
---
id: AS.M.002
name: GEPA (Genetic-Pareto Trajectory Evolution)
type: method
status: draft
source: arXiv 2507.19457, ICLR 2026 Oral
---

# AS.M.002: GEPA Framework для Self-Improvement

## Определение
GEPA (Genetic-Pareto) — метод самоулучшения автономных агентов через рефлексию на полных execution trajectories с Pareto-aware selection.

## Алгоритм (4 шага)

1. **Generate:** Агент генерирует N кандидатов (trajectories) для задачи
2. **Evaluate:** Запускает каждый, собирает full execution trace (inputs, tool calls, outputs, errors, reasoning)
3. **Reflect & Prune:** LLM читает траектории, диагностирует failure modes, отбирает Pareto-front (best по разным метрикам)
4. **Augment:** Комбинирует уроки с Pareto frontier, мутирует промпты/параметры, повторяет

## Отличие от RL
- **RL:** схлопывает trace → scalar reward → градиентный update
- **GEPA:** читает ПОЛНЫЙ trace (error messages, logs, intermediate steps) → natural language reflection → targeted fixes

## Metrics (2026)
- Обгоняет GRPO на 6-20%, используя 35x меньше rollouts
- Обгоняет MIPROv2 на 10%+ (AIME-2025: +12% accuracy)

## Применение в IWE
- Scout: учится на research trajectories (что искал → что нашёл → relevance feedback)
- Strategist: оптимизирует planning prompts на основе week review outcomes
- Любой agent с feedback loop

## Failure Modes
- **FM.001:** Pareto front вырождается в локальный оптимум (нет diversity) — решение: periodic random injection
- **FM.002:** Reflexion hallucination (агент "видит" несуществующие паттерны) — решение: ground reflection в actual logs
```

---

## 2. Production Autonomous Agents: ROI 200–500% за 6 месяцев

**Источник:** [OneReach AI: Agentic AI Stats 2026](https://onereach.ai/blog/agentic-ai-adoption-rates-roi-market-trends/), [Google Cloud: ROI of AI Agents](https://cloud.google.com/transform/roi-of-ai-how-agents-help-business)

**Релевантность:** ВЫСОКАЯ  
**Связь:** WP-144 (Автономные агенты), WP-142 (Fundraising — ROI benchmarks для pitch)

### Суть
**79% организаций** уже внедрили AI agents (не PoC, а production). **88%** планируют увеличить бюджет на агентов. **62%** ожидают ROI >100%. McKinsey: customer service/sales automation — **200–500% ROI за 6 месяцев**.

**Real case studies (2026):**
- **Amazon:** 25% faster delivery, 30% more-skilled roles (supply chain agents)
- **JPMorgan:** fraud detection agents на миллионах транзакций, автономная адаптация к новым угрозам
- **Ford:** vehicle design — processes from hours to seconds (sketch → 3D → stress analysis)
- **AtlantiCare:** clinical assistant, ambient note generation (healthcare admin)

**Failure rate:** Gartner — **40%+ agentic AI projects will be scrapped by 2027** не из-за моделей, а из-за operationalization (poor integration, unclear ownership, lack of production design).

### Reasoning
WP-144 Ф1-6 фокусируется на architecture + governance, но нет данных по **реальному ROI** и **failure modes in production**. Эти цифры нужны для:
1. **WP-142 (Fundraising):** Investors хотят benchmarks — "200-500% ROI в 6 мес" = сильный аргумент для seed pitch.
2. **AS.SOTA:** обновить SOTA.002 (autonomous agents) с 2026 production data.
3. **AS.FM:** новый failure mode — "operationalization gap" (40% projects fail NOT from tech, but from org design).

### Предложение
- **Тип:** SOTA-обновление + failure mode
- **Куда:** PACK-autonomous-agents → AS.SOTA.002 + AS.FM.008 (operationalization gap)
- **Действие:** Capture: Production Agents ROI + Operationalization Failure Mode → AS.SOTA + AS.FM

### Черновик для Экстрактора

```markdown
---
id: AS.SOTA.002
name: Autonomous Agents Production Deployment (2026)
type: sota
updated: 2026-03-23
---

# AS.SOTA.002: Production Autonomous Agents (2026 Update)

## Adoption Metrics
- 79% organizations deployed agents in production (not PoC)
- 88% planning budget increase for agentic capabilities
- 66% report measurable productivity improvements
- 62% expect ROI > 100%

## ROI Benchmarks
- **Customer service/sales automation:** 200–500% ROI within 6 months (McKinsey 2026)
- **Banking KYC/AML:** 200–2,000% productivity gains
- **Healthcare:** $150B annual savings potential by 2026
- **Supply chain:** Revenue growth 61% greater than peers with high AI investment

## Production Case Studies
| Company | Domain | Impact |
|---------|--------|--------|
| Amazon | Supply chain | +25% delivery speed, +30% skilled roles |
| JPMorgan | Finance | Fraud detection, millions of transactions, autonomous adaptation |
| Ford | Manufacturing | Design processes: hours → seconds |
| AtlantiCare | Healthcare | Clinical assistant, ambient notes |

## Failure Rate & Causes
- **40%+ projects scrapped by 2027** (Gartner)
- **NOT** model failure — operationalization gap:
  - Poor integration with existing systems
  - Unclear ownership (who manages agent lifecycle?)
  - Lack of production-grade design (monitoring, rollback, boundaries)

## Deployment Patterns (2026)
Mainstream adoption in constrained, well-governed domains:
- IT operations, employee service, finance ops
- Onboarding, reconciliation, support workflows
- Tolerate human-in-the-loop, clear boundaries, fast ROI

## Source
[OneReach AI Agentic AI Stats 2026](https://onereach.ai/blog/agentic-ai-adoption-rates-roi-market-trends/)  
[Google Cloud: ROI of AI Agents](https://cloud.google.com/transform/roi-of-ai-how-agents-help-business)

---
id: AS.FM.008
name: Operationalization Gap
type: failure_mode
---

# AS.FM.008: Operationalization Gap

## Описание
Агент работает в PoC/pilot, но **проваливается при переходе в production** НЕ из-за технических ограничений модели, а из-за организационных/инфраструктурных проблем.

## Признаки
- Pilot успешен, но prod deployment откладывается месяцами
- "Unclear ownership" — никто не хочет нести ответственность за агента
- Отсутствует мониторинг, rollback, границы автономии
- Агент конфликтует с legacy-системами (integration hell)

## Статистика
- 40%+ agentic AI projects scrapped by 2027 (Gartner 2026)
- Причина: NOT model failure — operationalization gap

## Mitigation
1. **Design for production first:** governance, monitoring, rollback BEFORE pilot
2. **Clear ownership:** назначить Product Owner для агента (как для микросервиса)
3. **Incremental integration:** не big-bang, а bounded pilots с чёткими KPI
4. **Production-grade infrastructure:** logs, metrics, circuit breakers с первого дня

## Связь с IWE
WP-144 Ф8 (deployment roadmap) должен учитывать этот failure mode: governance + monitoring + ownership BEFORE production.
```

---

## 3. WEF Trust Stack + Bounded Agency — новый стандарт governance

**Источник:** [WEF: How to design for trust in AI agents](https://www.weforum.org/stories/2026/02/how-to-design-for-trust-in-the-age-of-ai-agents/), [WEF: AI Agents in Action (2026)](https://www.weforum.org/publications/ai-agents-in-action-foundations-for-evaluation-and-governance/)

**Релевантность:** ВЫСОКАЯ  
**Связь:** WP-144 (governance Ф4), PACK-autonomous-agents AS.M (governance methods)

### Суть
WEF формализовал **Trust Stack** для автономных агентов (2026). Это НЕ чеклист, а **layered framework** с конкретными критериями на каждом уровне. **82% executives** планируют adopting agents в 1-3 года, но не знают как governance.

**5 компонентов Trust Stack:**
1. **Bounded Agency:** чёткие лимиты на действия агента, NO silent escalation
2. **Legible Reasoning Paths:** reasoning + limits + intentions видимы
3. **Goal Transparency:** явные objectives (accuracy vs safety vs engagement vs profit)
4. **Contestability & Override:** humans can challenge/correct/disengage легко (frictionless exit)
5. **Governance by Design:** logging, auditability, oversight embedded from day 1

**Progressive Governance:** по мере роста capability → safeguards расширяются пропорционально. Autonomy & authority = **adjustable design parameters**, не константы.

### Reasoning
WP-144 Ф4 (governance) упоминает Trust Stack абстрактно. Теперь WEF опубликовал **конкретный framework** (Feb 2026) с progressive governance approach. Это можно формализовать как **AS.M.003 (Trust Stack Method)** с пошаговым чеклистом для agent design.

Также важно для WP-142 (fundraising): показать investors что governance — не afterthought, а design principle.

### Предложение
- **Тип:** метод (governance framework)
- **Куда:** PACK-autonomous-agents → AS.M.003-trust-stack
- **Действие:** Capture: WEF Trust Stack → AS.M.003

### Черновик для Экстрактора

```markdown
---
id: AS.M.003
name: Trust Stack для Autonomous Agents (WEF 2026)
type: method
status: draft
source: WEF "AI Agents in Action" (2026)
---

# AS.M.003: Trust Stack — Governance Framework

## Определение
Layered trust framework для проектирования автономных агентов с progressive governance (safeguards растут пропорционально capability).

## 5 слоёв Trust Stack

### 1. Bounded Agency
- **Суть:** Чёткие лимиты на действия агента
- **Требование:** NO silent escalation (агент не может расширить свою autonomy без явного approval)
- **Пример:** Agent может read files, но не write БЕЗ подтверждения

### 2. Legible Reasoning Paths
- **Суть:** Reasoning + limits + intentions видимы для humans
- **Требование:** Agent объясняет ЧТО делает, ПОЧЕМУ, какие constraints учитывает
- **Пример:** "Searching codebase for error X, limiting to src/ folder, won't modify files"

### 3. Goal Transparency
- **Суть:** Явные objectives агента (не скрытые)
- **Требование:** User знает optimizes агент accuracy | safety | efficiency | engagement | profit
- **Пример:** "This agent optimizes for cost reduction, may suggest aggressive automation"

### 4. Contestability & Override
- **Суть:** Humans могут challenge/correct/disengage легко
- **Требование:** Frictionless exit (не "вы уверены? да/нет" x5 раз)
- **Пример:** Kill switch, rollback button, "stop and explain" command

### 5. Governance by Design
- **Суть:** Logging, auditability, oversight embedded с первого дня
- **Требование:** НЕ "добавим мониторинг потом" — governance = часть архитектуры
- **Пример:** Каждое действие → structured log (timestamp, reasoning, outcome, override)

## Progressive Governance
По мере роста capability агента → safeguards расширяются:
- **Low autonomy** (T0-T1): basic logging, human review
- **Medium autonomy** (T2): automated alerts, audit trails, oversight dashboard
- **High autonomy** (T3-T4): supervisor agents, anomaly detection, circuit breakers

Autonomy & Authority = **adjustable design parameters**, не константы.

## Применение в IWE
- Agent-card.yaml (WP-144 Ф4): добавить Trust Stack checklist
- Каждый новый агент проходит Trust Stack audit BEFORE production
- Progressive governance: Scout (T1) vs Strategist (T2) — разные safeguards

## Связь с AS.D.007
Дополняет AS.D.007 (Autonomy ≠ Automation) конкретным методом проектирования boundaries.

## Source
[WEF: How to design for trust in AI agents](https://www.weforum.org/stories/2026/02/how-to-design-for-trust-in-the-age-of-ai-agents/)  
[WEF: AI Agents in Action (2026)](https://www.weforum.org/publications/ai-agents-in-action-foundations-for-evaluation-and-governance/)
```

---

## 4. Failure Modes: Loop of Death + Self-Consistency Trap

**Источник:** [Medium: Loop of Death — 90% agents fail](https://medium.com/@sattyamjain96/the-loop-of-death-why-90-of-autonomous-agents-fail-in-production-and-how-we-solved-it-at-e98451becf5f), [Unite.AI: Hidden failure modes](https://www.unite.ai/the-ai-agents-trap-the-hidden-failure-modes-of-autonomous-systems-no-one-is-preparing-for/), [GitHub: Awesome Agent Failures](https://github.com/vectara/awesome-agent-failures)

**Релевантность:** КРИТИЧЕСКАЯ  
**Связь:** WP-144 (Ф5 monitoring), PACK-autonomous-agents AS.FM (failure modes)

### Суть
**Top production failure modes (2026):**

1. **Loop of Death (Runaway Agent):** Агент застревает в logic trap (try fix → fail → retry → fail), сжигая тысячи $ на API credits за минуты. **#1 killer** prod agents, not hallucination.

2. **Self-Consistency Trap:** Агент использует self-consistency (generate N outputs, majority vote) для mitigation, но **wrongly judges reasoning as consistent** despite logical contradictions. Внутренняя согласованность ≠ correctness.

3. **Goal Drift:** Agent начинает deviation from expected behavior (specification gaming, ethical boundary testing, logic corruption). Нужны supervisor agents.

4. **Hallucination in Action:** Когда LLM hallucinate → false text. Когда agent hallucinate → **false action** (modify files, run code, operate systems). Concrete failures.

5. **Data Drift & Corruption:** Agent не может прочитать receipt → fabricates plausible data (restaurant name, price) чтобы satisfy completion goal. Corrupts source of truth.

**Statistics:** 90% agents fail in production due to Loop of Death (не hallucination). GPTZero: 50+ papers с AI-fabricated citations в 300-paper sample ICLR 2026; 21% peer reviews = fully AI-generated.

### Reasoning
WP-144 Ф5 (monitoring) и AS.FM (failure modes) упоминают общие проблемы, но нет **конкретных** production failure modes с именами. Это gap — нужно формализовать:
- AS.FM.009: Loop of Death
- AS.FM.010: Self-Consistency Trap
- AS.FM.011: Hallucination in Action

Особенно критично для IWE scheduler.sh (WP-132) — timeout protection против Loop of Death уже есть, но нужен monitoring для goal drift.

### Предложение
- **Тип:** failure modes (3 новых)
- **Куда:** PACK-autonomous-agents → AS.FM.009, AS.FM.010, AS.FM.011
- **Действие:** Capture: Loop of Death + Self-Consistency Trap + Hallucination in Action → AS.FM

### Черновик для Экстрактора

```markdown
---
id: AS.FM.009
name: Loop of Death (Runaway Agent)
type: failure_mode
severity: critical
---

# AS.FM.009: Loop of Death (Runaway Agent)

## Описание
Агент застревает в бесконечном retry loop: пытается fix проблему → fails → retry → fails → retry, сжигая API credits/compute за минуты/часы.

## Признаки
- Rapidly increasing API calls (thousands per minute)
- Same action repeated with minimal variation
- No termination condition met
- Financial bleed (billing spike)

## Примеры
- Expense-reporting agent: не может прочитать receipt → fabricates data → validation fails → retry → fabricates again
- Code-fixing agent: syntax error → suggests fix → creates new error → fixes new error → creates third error...

## Почему возникает
- Отсутствие max_retries limit
- Completion goal без escape condition
- Agent не распознаёт "task impossible" — всегда пытается найти решение

## Mitigation
1. **Timeout на task-level:** hard limit (e.g., 5 min for single task)
2. **Max retries:** не больше N попыток (N=3-5)
3. **Divergence detection:** если каждый retry меняет > X% output → stop (не converging)
4. **Cost circuit breaker:** если billing > threshold/min → kill agent

## Статистика
- #1 killer of production agents (90% fail due to this, not hallucination)
- Financial impact: thousands $ in minutes

## Связь с IWE
- scheduler.sh (WP-132): timeout protection УЖЕ есть (TASK_TIMEOUT_LONG=1800), но нужен retry limit
- Scout: research task может loop если query не находит результаты → нужен max_iterations

## Source
[Medium: Loop of Death](https://medium.com/@sattyamjain96/the-loop-of-death-why-90-of-autonomous-agents-fail-in-production-and-how-we-solved-it-at-e98451becf5f)

---
id: AS.FM.010
name: Self-Consistency Trap
type: failure_mode
severity: medium
---

# AS.FM.010: Self-Consistency Trap

## Описание
Агент использует self-consistency (generate N outputs, majority vote) для повышения reliability, но **wrongly judges reasoning as consistent** despite logical contradictions. Внутренняя согласованность ≠ correctness.

## Признаки
- Multiple outputs converge to same (wrong) answer
- Agent confident (high self-consistency score), but output incorrect
- Logical contradictions invisible to agent (masked by surface agreement)

## Пример
- Запрос: "Какой год основания компании X?"
- Agent generates 5 answers: [2010, 2010, 2011, 2010, 2010]
- Majority vote: 2010 (high confidence)
- Actual: 2011 (одна правильная затерялась в шуме)

## Почему возникает
- Self-consistency работает для tasks с objective ground truth
- Ломается на ambiguous/under-specified tasks
- LLM bias: если context misleading → все N outputs будут consistent, но wrong

## Mitigation
1. **External validation:** не полагаться только на self-consistency — verify против source of truth
2. **Diversity check:** если все N outputs слишком похожи → red flag (likely bias)
3. **Human-in-the-loop** для high-stakes decisions

## Связь с AS.M.002 (GEPA)
GEPA решает это через Pareto frontier: не majority vote, а diversity across metrics.

## Source
[Unite.AI: Hidden failure modes](https://www.unite.ai/the-ai-agents-trap-the-hidden-failure-modes-of-autonomous-systems-no-one-is-preparing-for/)

---
id: AS.FM.011
name: Hallucination in Action
type: failure_mode
severity: critical
---

# AS.FM.011: Hallucination in Action

## Описание
Когда LLM hallucinates → false text (annoying, но обратимо). Когда autonomous agent hallucinates → **false action**: modifies files, runs commands, corrupts data. Concrete, irreversible failures.

## Признаки
- Agent выполняет действия на основе fabricated information
- Modifications to files/DB без ground truth validation
- "Plausible but wrong" outputs (agent заполняет пробелы в данных фантазией)

## Примеры
- Expense-reporting agent: не может прочитать receipt → fabricates restaurant name + price → corrupts accounting DB
- Code-refactoring agent: "assumes" function signature → renames vars → breaks API contract

## Почему критичнее text hallucination
- Text: user sees, can ignore/correct
- Action: already executed, requires rollback/cleanup
- Cascading failures: одна wrong action → breaks downstream systems

## Mitigation
1. **Read-only mode first:** agent explores, proposes changes, but doesn't execute until human approval
2. **Dry-run validation:** simulate action, show impact, get confirmation
3. **Rollback mechanism:** every action = transaction (can be undone)
4. **Source-of-truth anchoring:** before ANY write — verify data exists in authoritative source

## Связь с AS.M.003 (Trust Stack)
Bounded Agency (layer 1) должен prevent hallucination-driven actions через read/write separation.

## Source
[GitHub: Awesome Agent Failures](https://github.com/vectara/awesome-agent-failures)  
[Unite.AI: Hidden failure modes](https://www.unite.ai/the-ai-agents-trap-the-hidden-failure-modes-of-autonomous-systems-no-one-is-preparing-for/)
```

---

## 5. WISC Framework (Context Engineering) — operational standard 2026

**Источник:** [LangChain Blog: Context Engineering](https://blog.langchain.com/context-engineering-for-agents/), [GitHub: WISC Framework](https://github.com/coleam00/context-engineering-intro/tree/main/use-cases/ai-coding-wisc-framework)

**Релевантность:** ВЫСОКАЯ  
**Связь:** WP-144 (Ф3 memory), PACK-autonomous-agents AS.M (methods), DP (platform design)

### Суть
**Context Engineering заменил Prompt Engineering** как key discipline в 2026. WISC framework — 4 стратегии управления контекстом:

- **W (Write):** Externalize memory to files → survives context resets (persistence)
- **I (Isolate):** Sub-agents с отдельными контекстами → avoid noise pollution
- **S (Select):** Load only relevant context (RAG + grep), not everything
- **C (Compress):** Summarize long sessions OR hand off to fresh agent

**Why it matters:** Token accumulation = #1 problem для long-running agents. WISC = production-ready solution.

### Reasoning
WP-144 Ф3 (memory architecture) упоминает trajectory caching, но не детализирует **как именно** управлять context window в multi-turn sessions. WISC — конкретный framework, который:
1. **DP.M:** можно формализовать как platform-level pattern для всех агентов
2. **AS.M.004:** метод для agent design (Scout УЖЕ использует W — пишет в night-results/, но не использует I/S/C)
3. **WP-132 (scheduler):** Isolate strategy объясняет ЗАЧЕМ нужны отдельные агенты (не "просто модульность", а context isolation)

### Предложение
- **Тип:** метод (context management framework)
- **Куда:** PACK-autonomous-agents → AS.M.004-WISC + PACK-digital-platform → DP.M (cross-agent pattern)
- **Действие:** Capture: WISC Framework → AS.M.004 + DP.M

### Черновик для Экстрактора

```markdown
---
id: AS.M.004
name: WISC Framework (Context Engineering)
type: method
status: draft
source: LangChain Blog 2026, GitHub coleam00/context-engineering-intro
---

# AS.M.004: WISC Framework — Context Engineering for Agents

## Определение
Context Engineering = управление context window автономных агентов через 4 стратегии: Write, Isolate, Select, Compress. Заменил Prompt Engineering как ключевая дисциплина 2026.

## 4 стратегии

### W — Write (Externalize Memory)
- **Суть:** Сохранять context outside context window → available, но не жрёт tokens
- **Применение:** Agent пишет findings/plans/intermediate results в файлы
- **Пример (IWE):** Scout → night-results/, Strategist → DS-my-strategy/current/
- **Benefit:** Context survives resets, can be loaded selectively

### I — Isolate (Sub-Agents)
- **Суть:** Multiple agents с separate contexts вместо monolithic agent
- **Применение:** Task decomposition → каждый sub-agent видит только свою часть
- **Пример (IWE):** scheduler.sh → Scout | Strategist | Verifier (отдельные sessions)
- **Benefit:** Research noise из Scout НЕ pollutes Strategist context

### S — Select (Load Relevant Only)
- **Суть:** Подгружать только нужный context для текущей задачи
- **Применение:** RAG для semantic search + grep для exact matches
- **Пример (IWE):** Scout читает research-tasks.yaml (relevant), НЕ читает весь Pack
- **Benefit:** Reduces token consumption, improves focus

### C — Compress (Summarize or Hand Off)
- **Суть:** Когда session длинный → summarize ИЛИ hand off to fresh agent
- **Применение:** Post-process tool outputs (token-heavy search) | agent-agent handoff
- **Пример (IWE):** Week Review → Strategist summarizes 7 days → пишет итоги → NEW session для планирования
- **Benefit:** Prevents context window overflow

## Применение в IWE

| Agent | W | I | S | C |
|-------|---|---|---|---|
| Scout | ✅ night-results/ | ✅ отдельная session | ✅ читает research-tasks | ⚠️ нет (long sessions могут overflow) |
| Strategist | ✅ DS-my-strategy/ | ✅ отдельная session | ✅ MEMORY.md, DayPlan | ✅ Week Review → summary |
| Verifier | ⚠️ inline output | ✅ отдельная session | ✅ читает только artефact | ✅ short sessions (auto-compress) |

## Связь с другими методами
- **AS.M.001 (Improvement Loop):** WISC Write стратегия = persistence для trajectory caching
- **AS.M.003 (Trust Stack):** Isolate = context isolation между агентами (governance)
- **DP.IPO:** WISC = cross-agent pattern (можно формализовать в DP.M)

## Gap в текущей IWE
- Scout: использует W, I, S, но НЕ C → long research sessions могут overflow context
- Решение: добавить Compress strategy — если session > 100K tokens → summarize findings, start fresh

## Source
[LangChain Blog: Context Engineering](https://blog.langchain.com/context-engineering-for-agents/)  
[GitHub: WISC Framework](https://github.com/coleam00/context-engineering-intro/tree/main/use-cases/ai-coding-wisc-framework)
```

