# Capture Candidates — 2026-03-23

> Черновики для Экстрактора (R2). Готовые формулировки для Pack-сущностей.

---

## Различение: Autonomy ≠ Automation (AS.D.007 update)

**Источник:** WEF "How to design for trust in AI agents" (2026)

### Черновик

```markdown
---
id: AS.D.007
name: Автономия ≠ Автоматизация
type: distinction
updated: 2026-03-23
source: WEF 2026
---

# AS.D.007: Автономия ≠ Автоматизация

## Суть различения
WEF определяет **автономию** как "гибкость принятия решений" (agent выбирает ЧТО делать), а **автоматизацию** как "надёжное исполнение" (система делает ЧТО запрограммировано). Это **разные классы систем** с разными требованиями к governance.

## Таблица сравнения

| Параметр | Автономия | Автоматизация |
|----------|-----------|----------------|
| **Определение** | Гибкость принятия решений | Надёжное исполнение |
| **Вопрос** | ЧТО делать? | КАК делать? |
| **Decision-making** | Agent выбирает из множества опций | Фиксированный алгоритм |
| **Governance** | Trust Stack, bounded agency, oversight | Validation, error handling, monitoring |
| **Failure mode** | Goal drift, hallucination in action | Logic bugs, edge cases |
| **Пример (IWE)** | Scout (исследует → решает что релевантно) | scheduler.sh (выполняет tasks по расписанию) |
| **Требования** | Legible reasoning, contestability, rollback | Idempotency, retries, logging |

## Импликации для проектирования
- **Автоматизация:** focus на reliability (тесты, мониторинг, graceful degradation)
- **Автономия:** focus на governance (boundaries, transparency, human override)

Попытка применить governance для автоматизации → overengineering.  
Попытка применить automation patterns для автономии → underspecified (agent выходит за рамки).

## Связь с IWE
- **scheduler.sh (WP-132):** автоматизация → нужны timeouts, retry limits, logs
- **Новые агенты (Scout, Strategist):** автономия → нужны Trust Stack, bounded agency

## Upstream
- SPF.D: общее различение "решение vs исполнение"
- AS.M.003: Trust Stack = governance для автономии

## Source
[WEF: How to design for trust in AI agents](https://www.weforum.org/stories/2026/02/how-to-design-for-trust-in-the-age-of-ai-agents/)
```

---

## SOTA: Fundraising (ECO.M.001 — новый метод)

**Источник:** YC, a16z, Qubit Capital, множественные seed guides 2026

### Черновик

```markdown
---
id: ECO.M.001
name: Fundraising для EdTech Seed ($5-15M)
type: method
status: draft
created: 2026-03-23
source: YC Seed Deck Guide, Qubit Capital, Flowjam Seed Valuation 2026
---

# ECO.M.001: Fundraising Best Practices (EdTech Seed 2026)

## Scope
Метод привлечения seed-инвестиций ($5-15M) для EdTech/Intelligence Development платформ. Horizon: 2024-2026 data.

## Контекст 2026
- **Seed funding DOWN 29% YoY** (fewer deals)
- **Median valuation UP 19% → $16M pre-money** (premium for winners)
- **Bar raised:** seed expectations = old Series A (need $300K-$500K ARR)
- **Investor psychology:** "data-driven storytelling", not "growth at all costs"

## 5 компонентов метода

### 1. Investor Psychology (что смотрят первым)
**Top priorities (YC, a16z, Sequoia 2026):**
- **Traction > Deck:** ARR/users/engagement metrics, not slides
- **Bottom-up model:** realistic projections (segment × conversion × ARPU), not top-down TAM
- **Team credibility:** domain expertise (EdTech experience OR adjacent success)
- **Problem-solution fit:** clear pain point, not "nice to have"

**Typical rejection reasons:**
- No traction ($0 ARR at seed = red flag 2026)
- Top-down financials ("we'll capture 1% of $100B market")
- Vague differentiation ("AI-powered learning platform")

### 2. Структура сделки (Seed $5-15M)
**2026 standards:**
- **Instrument:** SAFE most common (simpler than priced round), convertible note rare
- **Valuation cap:** $16M median pre-money (2026), higher for AI/EdTech with traction
- **Discount:** 15-20% typical for SAFE
- **Pro-rata rights:** investors want право на follow-on в Series A

**Term sheet essentials:**
- Founder-friendly: no board seats at seed (observer rights ok)
- Vesting: 4-year with 1-year cliff (standard)
- Liquidation preference: 1x (not participating preferred)

### 3. Bottom-Up Projections (anti-pattern: top-down)
**Framework:**
```
ARR = Segments × Conversion × ARPU × Retention
```

**Example (IWE):**
- Segment 1 (Individual learners): 10K trials → 3% conv → $10/mo → 12 mo retention = $36K ARR
- Segment 2 (B2B teams): 100 trials → 15% conv → $100/mo → 24 mo retention = $36K ARR
- Total Year 1: $72K ARR

**Then project growth:**
- Year 2: 5x trials (proven channel) → $360K ARR
- Year 3: 3x ARPU (tier expansion) → $1M ARR

**Investors validate:**
- Conversion % (benchmark: freemium EdTech 3-5%)
- ARPU (benchmark: Coursera $50-200/user/year)
- Retention (benchmark: B2B SaaS 90% NDR)

### 4. Competitor Positioning (EdTech)
**Matrix (IWE vs Coursera/Udemy/Khan):**

| Dimension | Coursera | Udemy | Khan Academy | **IWE (Aisystant)** |
|-----------|----------|-------|--------------|---------------------|
| Content | Courses (3rd party) | Courses (marketplace) | Free K-12 | **Methodology (FPF)** |
| Personalization | Recommendations | Search | Adaptive practice | **Digital Twin** |
| Community | Forums | Q&A | None | **13K practitioners** |
| Business model | B2C + B2B | B2C + B2B | Donations | **Freemium + B2B** |
| AI | Content recommendations | Search | Adaptive quizzes | **Autonomous agents (exocortex)** |

**Positioning statement:**
"Coursera/Udemy = content delivery. IWE = **intelligence operating system**. They sell courses; we build **созидатели** through methodology + digital twin + AI agents."

### 5. Pitch Deck Structure (YC template 2026)
**12 slides (10-15 min):**
1. **Problem:** созидатели тонут в complexity (системное мышление недоступно)
2. **Solution:** IWE ecosystem (methodology + platform + community + созидатель)
3. **Why now:** AI agents = exoskeleton (усиливают мышление, не заменяют)
4. **Market:** EdTech $44.5B by 2026, niche = adult professional development
5. **Business model:** Red Hat (open methodology, paid infrastructure)
6. **Traction:** 13K users, $X ARR, Y% MoM growth
7. **Product:** Digital Twin + Agents + Руководства (screenshots)
8. **Competition:** Matrix (см. выше)
9. **Go-to-market:** Community-led growth (not paid ads)
10. **Financials:** Bottom-up ARR projection (3 years)
11. **Team:** Founders + advisors (domain credibility)
12. **Ask:** $10M seed at $16M pre-money, 18-month runway

## Failure Modes
- **FM.001:** Top-down TAM ("$100B market") без bottom-up validation → investors dismiss
- **FM.002:** No traction at seed → "come back when you have $500K ARR"
- **FM.003:** Unclear differentiation → "this is just another Coursera clone"
- **FM.004:** Team без EdTech credibility → "why should YOU win this?"

## Применение к WP-142
1. **Ф2 (financial model):** использовать bottom-up, не top-down
2. **Ф3 (pitch deck):** YC 12-slide template
3. **Ф4 (investor outreach):** target EdTech-специфичные VCs (не general tech funds)

## Benchmarks (2026)
- **Seed valuation:** $16M pre-money median
- **ARR at seed:** $300K-$500K expected (2026 bar)
- **Freemium conversion:** 3-5% (EdTech baseline)
- **B2B ACV:** $10K-$50K (team tier)
- **NDR (B2B):** 90-110% (expansion через upsell)

## Sources
- [YC: How to build seed pitch deck](https://www.ycombinator.com/library/2u-how-to-build-your-seed-round-pitch-deck)
- [Qubit Capital: EdTech Series A Pitch](https://qubit.capital/blog/edtech-series-a-pitch)
- [Flowjam: Seed Valuation 2025 Guide](https://www.flowjam.com/blog/seed-round-valuation-2025-complete-founders-guide)
```

---

## SOTA: AI Agents + Smart Contracts (новая область, AS или отдельный Pack?)

**Источник:** Coinbase x402, Olas, Medium "AI Agents + Blockchain" 2026

### Черновик

```markdown
---
id: AS.SOTA.003
name: AI Agents × Smart Contracts (Decentralized Autonomy)
type: sota
status: draft
created: 2026-03-23
source: Coinbase x402, Olas Governatooorr, Medium 2026
---

# AS.SOTA.003: AI Agents × Smart Contracts — Symbiosis (2026)

## Суть тренда
В 2026 происходит convergence AI agents + blockchain smart contracts. Blockchain даёт agents **native economic agency** (собственный wallet, payment rails), а AI даёт smart contracts **adaptive intelligence** (не просто "if X then Y", а decision-making).

## Ключевые события 2026

### Production Infrastructure
- **Coinbase Agentic Wallets:** 50M+ machine-to-machine transactions (M2M payments)
- **Alchemy x402 protocol (Mar 2026):** Agent с wallet получает HTTP 402 payment request → автоматически tops up USDC на Base → оплачивает API без human input
- **Market cap AI agent tokens:** $7.7B, daily volume $1.7B

### Real Use Cases
1. **DeFi Risk Management:** AI agents run 24/7 monitoring (collateral risk, liquidity pools, abnormal transactions). TVL institutional DeFi > $95B (Feb 2026).
2. **Autonomous Governance:** Olas Governatooorr — AI delegate голосует on-chain (Ethereum, Solana proposals) без human.
3. **Trading Agents:** Walbi (Mar 9, 2026) — no-code AI trading agents для retail. User описывает strategy plain language → agent executes.

## Архитектура

### Traditional Smart Contract (до 2026)
```
IF condition THEN action
```
Фиксированная логика, нет адаптации.

### AI-Powered Smart Contract (2026)
```
AI Agent observes data → ML predicts risk → Smart Contract adapts parameters
```
Adaptive contracts: используют ML для предсказания рисков, автономно оптимизируют execution на основе real-time data.

### Decentralized Multi-Agent Systems
- Agents с отдельными wallets (identity + payment source)
- Communicate через smart contracts (on-chain messaging)
- Execute financial transactions autonomously (no human in loop)

## Проблемы и риски (2026)

### Вызовы
- **Accountability gap:** кто отвечает если agent теряет деньги? (autonomy vs responsibility)
- **$17B in AI-assisted scam losses** (2026) — agents используются для fraud
- **Отсутствие kill switches:** нет стандартов для emergency shutdown

### Governance вопросы
Balance **autonomy** (agent принимает решения) vs **accountability** (human ответственность). WEF Trust Stack применяется, но decentralized governance = сложнее (нет central authority).

## Релевантность для IWE
**Fleeting-note (22 мар):** "Симбиоза автономных ИИ-агентов и смарт-контрактов. На поверхности тема лежит, но почему-то практически ничего не слышно."

**Ответ (2026):** Уже НЕ "ничего не слышно" — это production reality:
- 50M+ transactions (Coinbase)
- $7.7B market cap
- Real use cases (DeFi, governance, trading)

**Применение к IWE:**
- Пока НЕ приоритет (фокус на agents for intelligence work, не finance)
- Но потенциал: monetization через crypto (micropayments за AI services)
- Governance паттерны (Trust Stack) applicable к decentralized agents

## Куда записать?
- Если создаём новый Pack для Web3/Decentralized AI → новый Pack-ID
- Если расширяем AS → AS.SOTA.003
- Рекомендация: **пока AS.SOTA.003** (watching brief), если станет core area → выделить Pack

## Sources
- [Medium: AI Agents + Blockchain](https://medium.com/coinmonks/ai-agents-blockchain-how-autonomous-web3-systems-are-shaping-the-future-of-the-internet-926b262fc323)
- [Outlook India: AI Agents in DeFi](https://www.outlookindia.com/xhub/blockchain-insights/ai-agents-in-defi-how-autonomous-risk-systems-transform-liquidity-collateral-and-smart-contract-security)
- [Millionero: AI Agents in Crypto 2026](https://blog.millionero.com/blog/ai-agents-in-crypto-how-autonomous-finance-is-becoming-real-in-2026/)
```

