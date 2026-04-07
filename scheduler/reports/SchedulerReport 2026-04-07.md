---
type: scheduler-report
date: 2026-04-07
week: W15
agent: Синхронизатор
---

# Отчёт планировщика: 2026-04-07

## 🔴 Критический сбой — требуется внимание

> **Замечания:** code-scan не запустился; strategist morning не запустился; 

## Результаты

| # | Задача | Статус | Время |
|---|--------|--------|-------|
| 1 | Сканирование кода | **❌** | — |
| 2 | Стратег утренний | **❌** | — |
| 3 | Разбор заметок | **❌** | — |
| 5 | Проверка входящих | **✅** | 259121 сек назад |

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

Нет ошибок. ✅
