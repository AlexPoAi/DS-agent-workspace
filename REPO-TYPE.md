# Тип репозитория

**Тип**: `DS/governance`

**Source-of-truth**: no

## Роль

Шина данных автономных агентов IWE. Хранит выходные артефакты агентов (черновики, отчёты, находки). Потребители: человек, Экстрактор, Стратег, другие агенты.

## Принцип

Автор-агент → этот репозиторий. Автор-человек → DS-my-strategy.
Черновик агента ≠ дубль утверждённого документа. Это разные артефакты: raw vs approved.

## Upstream dependencies

- [TserenTserenov/DS-autonomous-agents](https://github.com/TserenTserenov/DS-autonomous-agents) — код агентов (instrument)
- [TserenTserenov/PACK-autonomous-agents](https://github.com/TserenTserenov/PACK-autonomous-agents) — source-of-truth проектирования агентов
- [TserenTserenov/PACK-digital-platform](https://github.com/TserenTserenov/PACK-digital-platform) — source-of-truth архитектуры платформы

## Downstream outputs

- DS-my-strategy — утверждённые документы (DayPlan, captures)
- PACK-* — формализованные captures (через Экстрактора)

## Non-goals

- НЕ содержит кода агентов (→ DS-autonomous-agents)
- НЕ содержит trajectory cache (→ DS-autonomous-agents, часть кода)
- НЕ содержит утверждённых решений человека (→ DS-my-strategy)
