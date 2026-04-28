---
type: scheduler-report
date: 2026-04-28
week: W18
agent: Синхронизатор
---

# Отчёт планировщика: 2026-04-28

## 🔴 Критический сбой — требуется внимание

> **Замечания:** strategist morning не запустился; 

## Результаты

| # | Задача | Статус | Время |
|---|--------|--------|-------|
| 1 | Сканирование кода | **✅** | 00:04:37 |
| 2 | Стратег утренний | **❌** | 11:47:28 |
| 5 | Проверка входящих | **🟡** | 02:23:05 |

## Runtime mode

- Provider primary: **codex**
- Provider reason: **both_available_preference_codex**
- Codex: **available** (`login_ok`)
- Claude: **available** (`auth_status_ok`)
- Runtime policy: **split**
- Local control plane: **available** (`launchctl_scheduler_loaded`)
- Cloud RAG: **unknown** (`health_url_not_configured`)
- Cloud takeover scope: **product-only**
- Cloud bot runtime: **vps**
- Runtime policy file: **present**
- Runtime mode artifact: **present**

## Ошибки и предупреждения

- Стратег: открытие дня: codex provider failed- Синхронизатор: отчёт планировщика: статус устарел для текущего окна- Экстрактор: проверка входящих: статус устарел для текущего окна

**Что делать:**
