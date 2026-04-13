---
type: scheduler-report
date: 2026-04-09
week: W15
agent: Синхронизатор
---

# Отчёт планировщика: 2026-04-09

## 🔴 Критический сбой — требуется внимание

> **Замечания:** strategist morning не запустился; 

## Результаты

| # | Задача | Статус | Время |
|---|--------|--------|-------|
| 1 | Сканирование кода | **✅** | 04:27:31 |
| 2 | Стратег утренний | **❌** | 07:27:12 |
| 5 | Проверка входящих | **🟡** | 21:00:05 |

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

- Стратег: открытие дня: claude-compatible provider failed- Стратег: разбор заметок: claude-compatible provider failed- Синхронизатор: отчёт планировщика: статус устарел для текущего окна- Экстрактор: проверка входящих: статус устарел для текущего окна

**Что делать:**
