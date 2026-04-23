---
skill_id: environment-engineer.fpf-srt-spf-domain-ritual
agent: environment-engineer
status: active
version: 1.0
created: 2026-04-20
updated: 2026-04-20
migration_target: /Users/alexander/Github/DS-strategy/PACK-agent-skills/03-skills/AGENT.SKILL.001-engineer-ritual-sequence (Скилл инженера: ритуал реализации инженерной задачи).md
---

# Skill: FPF -> SRT -> SPF Domain Ritual

## Зачем нужен

Этот skill нужен для ситуаций, когда инженер среды или инфраструктурный разработчик:

- создаёт новый домен или поддомен;
- перестраивает существующий domain layer;
- формализует skill/agent/runtime слой вокруг нового доменного контура;
- хочет избежать дрейфа, когда domain/subdomain задаются интуитивно, а не через методологическую цепочку.

Источник связки уже materialized в живом контуре:

- `WP-87` зафиксировал формулу:
  - `FPF` как различение;
  - `SRT` как раскладка;
  - `SPF` как formalization.
- `WP-90` дополнительно закрепил guardrail:
  - `SRT` — это placement layer, а не source-of-truth для domain/subdomain boundaries.

## Когда запускать

- Когда нужно определить домен
- Когда нужно определить поддомен
- Когда создаётся skill/agent под новый доменный контур
- Когда инфраструктурный change затрагивает domain map, registry layer или boundaries

## Не запускать когда

- Нужен быстрый локальный bug-fix без изменения domain layer
- Нужна только техническая починка скрипта или runtime
- Domain/subdomain уже доказаны и нужно лишь внести маленькую правку

## Каноническая последовательность

### Шаг 1 — `FPF`: различи сущность

Сначала ответь на вопросы:

- что это за сущность;
- почему это отдельный домен;
- почему это не часть уже существующего домена;
- где проходит граница;
- что здесь является source-of-truth.

На этом шаге запрещено:

- подбирать слот в `SRT` раньше, чем различён домен;
- формализовывать документы до различения границ;
- называть поддомен только потому, что он “удобно звучит”.

Результат шага:

- различены domain и subdomain boundaries;
- отделено “ядро домена” от соседних контуров;
- зафиксировано, что именно здесь делает агент/роль.

### Шаг 2 — `SRT`: разложи уже различённое

Только после `FPF`:

- размести domain / subdomain / entry в рабочей `SRT`-матрице;
- используй `SRT` как placement layer;
- проверь, не маскирует ли `SRT` неясность, вместо того чтобы помогать раскладке.

Принцип:

- `SRT` помогает раскладывать уже различённое;
- `SRT` не доказывает существование домена;
- `SRT` не назначает domain/subdomain boundaries сам по себе.

Результат шага:

- у различённого домена есть `srt placement`;
- видно, как он встраивается в систему;
- спорные места помечены `needs-review`, а не форсированы.

### Шаг 3 — `SPF`: формализуй в артефакты

Только после `FPF` и `SRT`:

- создавай role-card;
- создавай skill;
- создавай WP context;
- создавай registry layer;
- описывай процессы, входы, выходы и acceptance.

Здесь `SPF` отвечает за formalization:

- что именно materialized;
- в каких файлах это хранится;
- какие поля обязательны;
- какой acceptance у результата.

Результат шага:

- domain/subdomain не только различены, но и оформлены в повторяемый артефактный контур.

## Короткая формула

`FPF` -> различи  
`SRT` -> разложи  
`SPF` -> формализуй

Или ещё короче:

- `FPF` = boundaries
- `SRT` = placement
- `SPF` = artifact contract

## Quality gates

- Не определять домен через `SRT`
- Не превращать `SRT` в source-of-truth
- Не прыгать сразу в документ/skill без `FPF` различения
- Не смешивать domain decision и technical implementation
- Если граница спорна, остановиться на `needs-review`, а не симулировать ясность

## Шаблон результата

```markdown
# FPF -> SRT -> SPF pass

## FPF
- domain:
- subdomain:
- boundary:
- not-this-domain:
- source-of-truth:

## SRT
- placement:
- why here:
- doubtful placements:

## SPF
- role-card:
- skill:
- registry / wp / context:
- acceptance:

---
Skill: fpf-srt-spf-domain-ritual
Agent: Environment Engineer
Date: [YYYY-MM-DD]
Status: [draft / ready / needs-review]
```

## Связь с другими артефактами

- `DS-strategy/archive/wp-contexts/WP-87-park-domain-subdomain-map-for-curator (Park domain-subdomain map для Knowledge Registry Curator).md`
- `DS-strategy/archive/wp-contexts/WP-90-knowledge-registry-curator-srt-aware-upgrade (SRT-aware апгрейд Knowledge Registry Curator).md`
- `DS-strategy/PACK-agent-skills/03-skills/AGENT.SKILL.001-engineer-ritual-sequence (Скилл инженера: ритуал реализации инженерной задачи).md`
