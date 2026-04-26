---
type: scheduler-report
date: 2026-04-25
week: W17
agent: Синхронизатор
---

# Отчёт планировщика: 2026-04-25

## 🔴 Критический сбой — требуется внимание

> **Замечания:** strategist morning не запустился; 

## Результаты

| # | Задача | Статус | Время |
|---|--------|--------|-------|
| 1 | Сканирование кода | **✅** | 00:00:03 |
| 2 | Стратег утренний | **❌** | 06:36:53 |
| 5 | Проверка входящих | **✅** | 03:53:25 |

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

- Стратег: открытие дня: codex provider failed- Синхронизатор: отчёт планировщика: статус устарел для текущего окна

**Что делать:**
