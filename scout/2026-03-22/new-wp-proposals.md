# New WP Proposals — 2026-03-22

> Идеи для новых РП, обнаруженные в ходе разведки

---

## WP-NEW-1: Verifiable Credentials для квалификаций (ЦД 4.0)

**Источник:** fleeting-notes.md (22 мар, 21:54 — заметка про Truvity CTO)

**Суть:**
Использовать W3C Verifiable Credentials (VC) для хранения квалификаций пользователя IWE. Aisystant становится **Issuer VC** — выдаёт криптографически подписанные attestations о завершении ступеней, курсов, ассессментов. Пользователь хранит VC в своём кошельке (EUDI Wallet для EU, или custodial wallet для RU/CIS), предъявляет при входе на платформу или работодателю.

**Преимущества:**
- **Portability:** пользователь владеет своими credentials, не платформа
- **Verifiability:** любой может проверить подпись Aisystant без обращения к API
- **Interoperability:** VC можно использовать на других платформах, в смарт-контрактах, для job applications
- **Trust:** платформа не может "молча удалить" достижение пользователя (как с записью в БД)

**Технический стек:**
- Truvity API (infrastructure для VC lifecycle: issue, verify, revoke)
- W3C Verifiable Credentials standard
- DID (Decentralized Identifiers)
- Integration с Ory Kratos (trait: "kyc_verified", "qualification_level_N")

**Зависимости:**
- WP-118 (Ory Hydra/Kratos setup) — нужна аутентификация как основа
- WP-116 (ЦД v3.0) — нужна метамодель характеристик для определения что certify

**Связь с Pack:**
- PACK-digital-platform: новый компонент платформы (VC issuer service)
- PACK-ecosystem: модель monetization через credentials (B2B use case — employers verify qualifications)

**Горизонт:** Q3-Q4 2026 (после Ory + ЦД v3.0)

**Риски:**
- EUDI Wallet adoption в EU медленный (2026 pilot only)
- Для RU/CIS аудитории менее актуально (нет government VC infrastructure)
- Юридическая валидность VC как proof of qualification (регуляторная неопределённость)

**Стратегический фит:**
- S2 (международный выход): VC критичен для EU market (eIDAS 2.0 compliance)
- S1 (масштабирование): B2B use case (employers verify без API calls → снижение инфраструктурной нагрузки)

---

## WP-NEW-2: AI Agents × Smart Contracts Symbiosis

**Источник:** fleeting-notes.md (22 мар, 15:31 — "симбиоза автономных ИИ-агентов и смарт-контрактов")

**Суть:**
Исследовать и прототипировать интеграцию автономных ИИ-агентов с смарт-контрактами (blockchain). Use cases:
1. **Agent-triggered contracts:** агент выполняет задачу → автоматически вызывает смарт-контракт (e.g., выплата reward за завершение ступени)
2. **Contract-supervised agents:** смарт-контракт как governance layer для агента (bounded autonomy enforced on-chain)
3. **Decentralized agent marketplace:** агенты предлагают услуги, смарт-контракты обеспечивают escrow + reputation

**Технический стек:**
- Ethereum / Polygon для смарт-контрактов (низкий gas)
- Agent framework (LangGraph или CrewAI)
- Web3.py для interaction
- IPFS для storing agent outputs (off-chain data)

**Зависимости:**
- WP-132 (Scheduler.sh → автономные агенты) — нужна база автономных агентов
- WP-144 (Autonomous agents strategy) — нужна стратегия применения

**Связь с Pack:**
- PACK-autonomous-agents: новый архитектурный паттерн (agents + blockchain)
- PACK-ecosystem: новый monetization model (agent marketplace, tokenized incentives)

**Горизонт:** Q4 2026 — Q1 2027 (research + PoC)

**Риски:**
- Blockchain hype vs substance (большинство use cases не требуют blockchain)
- Gas costs для frequent agent→contract interactions
- Regulatory uncertainty (crypto regulation 2026-2027)

**Стратегический фит:**
- S7 (прорывные технологии): Web3 + AI agents = frontier
- S6 (финансы): tokenized economy модель (альтернативная monetization)

**Exploration Task:**
Добавить в research-tasks.yaml как rt-005:
```yaml
- id: rt-005
  title: "Symbiosis of autonomous AI agents and smart contracts: use cases, architecture, SOTA"
  context: >
    Investigate integration patterns for AI agents × blockchain smart contracts.
    Focus: (1) Real production use cases (not PoC/demos), (2) Architectural patterns
    (agent-triggered contracts, contract-supervised agents, decentralized marketplaces),
    (3) Technical feasibility (gas costs, latency, security), (4) Regulatory landscape.
    Result → Pack-knowledge для PACK-autonomous-agents + PACK-ecosystem.
  agent: scout
  wp: TBD (new WP после одобрения)
  priority: low (exploration phase)
  added: 2026-03-22
  status: pending
```

