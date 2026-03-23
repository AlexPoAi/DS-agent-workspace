# New WP Proposals — 2026-03-23

> Идеи для новых рабочих продуктов на основе находок.

---

## WP: Context Compression Strategy для Scout

**Тип:** enhancement (WP-144 sub-task)  
**Приоритет:** medium  
**Связь:** AS.M.004 (WISC Framework), находка #5

### Проблема
Scout использует W (Write), I (Isolate), S (Select) из WISC, но НЕ использует **C (Compress)**. Long research sessions (6-8 source scans) могут overflow context window → degraded performance в конце session.

### Решение
Добавить Compress strategy:
1. **Threshold:** если session > 100K tokens → trigger compress
2. **Mechanism:** summarize findings so far → write to morning-ideas.md (partial) → start fresh context
3. **Hand-off:** новый agent читает summary + продолжает research

### Фазы (оценка: 4h)
- Ф1 (1h): Implement token counter в Scout session
- Ф2 (2h): Compress mechanism (summarize + hand-off logic)
- Ф3 (1h): Test на long research task (rt-003 fundraising = good candidate)

### Outcome
Scout может run longer research tasks без context overflow. Качество находок в конце session = качеству в начале.

---

## WP: Production Agent Failure Monitoring Dashboard

**Тип:** new infrastructure  
**Приоритет:** high (если WP-144 Ф5 monitoring выходит в prod)  
**Связь:** AS.FM.009-011 (Loop of Death, Self-Consistency Trap), находка #4

### Проблема
Failure modes (Loop of Death, Goal Drift, Hallucination in Action) не мониторятся proactively. Сейчас обнаруживаются post-factum (billing spike, corrupted data).

### Решение
Dashboard для real-time monitoring agent health:
- **Loop Detection:** API calls/min > threshold → alert
- **Goal Drift:** output semantic similarity к expected baseline < threshold → flag
- **Cost Circuit Breaker:** billing > $X/hour → auto-pause agent
- **Action Log:** all write operations (files, DB, API calls) → audit trail

### Фазы (оценка: 16h)
- Ф1 (4h): Design metrics (что мониторить, thresholds)
- Ф2 (6h): Instrumentation (agents send telemetry to dashboard)
- Ф3 (4h): Dashboard UI (Grafana или custom)
- Ф4 (2h): Alerts (Telegram notifications on anomalies)

### Outcome
Proactive failure detection. Loop of Death caught в первые 5 минут (не через час когда billing spike). Goal Drift visible через metrics, не через corrupted output.

---

## WP: Fundraising Pitch Deck v1.0 (WP-142 accelerator)

**Тип:** new artifact  
**Приоритет:** high (Q2 goal: fundraising prep)  
**Связь:** ECO.M.001 (Fundraising method), находка #2 (ROI benchmarks)

### Проблема
WP-142 Ф3 (pitch deck) запланирован, но нет конкретного метода. Находка #2 (ECO.M.001) даёт YC 12-slide template + bottom-up model + competitor positioning.

### Решение
Создать pitch deck v1.0 на основе ECO.M.001:
- **Slides 1-3:** Problem (созидатели тонут в complexity) + Solution (IWE ecosystem) + Why now (AI agents)
- **Slides 4-6:** Market ($44.5B EdTech) + Business model (Red Hat) + Traction (13K users, ARR)
- **Slides 7-9:** Product (Digital Twin screenshots) + Competition (matrix vs Coursera/Udemy) + GTM (community-led)
- **Slides 10-12:** Financials (bottom-up ARR 3 years) + Team (credibility) + Ask ($10M seed)

### Фазы (оценка: 12h)
- Ф1 (2h): Data collection (ARR, MoM growth, user metrics)
- Ф2 (4h): Bottom-up financial model (segment × conversion × ARPU)
- Ф3 (4h): Slide design (Figma или Google Slides)
- Ф4 (2h): Review cycle (feedback от advisors)

### Outcome
Pitch deck v1.0 готов для investor meetings. Alignment с YC best practices → выше вероятность positive response.

