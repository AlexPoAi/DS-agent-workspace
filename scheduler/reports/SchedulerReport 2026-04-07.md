---
type: scheduler-report
date: 2026-04-07
week: W15
agent: Синхронизатор
---

# Отчёт планировщика: 2026-04-07

## 🔴 Критический сбой — требуется внимание

> **Замечания:** strategist morning не запустился; note-review не запустился; 

## Результаты

| # | Задача | Статус | Время |
|---|--------|--------|-------|
| 1 | Сканирование кода | **✅** | 23:05:54 |
| 2 | Стратег утренний | **❌** | — |
| 3 | Разбор заметок | **❌** | — |
| 5 | Проверка входящих | **✅** | 259626 сек назад |

## Runtime mode

- Provider primary: **claude**
- Provider reason: **only_claude_available**
- Codex: **missing** (`codex_cli_not_found`)
- Claude: **available** (`auth_helper_ok`)
- Runtime policy: **split**
- Local control plane: **available** (`launchctl_scheduler_loaded`)
- Cloud RAG: **unknown** (`health_url_not_configured`)
- Cloud takeover scope: **product-only**
- Cloud bot runtime: **declared**
- Runtime policy file: **present**
- Runtime mode artifact: **present**

## Ошибки и предупреждения

- [2026-04-07 23:05:54] [scheduler] WARN: strategist note-review failed (will retry next dispatch)
- [2026-04-07 23:05:59] [scheduler] WARN: extractor inbox-check failed (will retry next dispatch)

**Что делать:**
