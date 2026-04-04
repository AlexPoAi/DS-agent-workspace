---
type: engineering-context
created: 2026-04-04
updated: 2026-04-04
engineer: Claude Sonnet 4.6
---

# Контекст: история инженерных работ по scheduler/strategist

> Накопленное знание о системе. Читать перед любым РП по экзокортексу.

---

## Хронология инцидентов (все связанные с scheduler/strategist)

| Дата | WP | Инцидент | Корень | Фикс |
|------|----|----------|--------|------|
| 2026-03-04 | WP-2 | Агенты не работали после простоя | OAuth 401 истёк, нет алерта | health-check.sh |
| 2026-03-19 | WP-20 | 10 дней без агентов | OAuth 401 без уведомления | run_claude детектор |
| 2026-03-20 | WP-21 | Нет мониторинга агентов | Нет health-check и статус-файлов | AGENTS-STATUS.md + health-check |
| 2026-03-21 | WP-22-27 | Стабилизация рабочей среды | Truthful semantics, close-flow | scheduler truthful reporting |
| 2026-03-22 | WP-26-27 | day-close зависает как директория | lock создаётся как dir, trap не срабатывает | stale lock 22.03 (не устранён до 04.04) |
| 2026-03-28 | ENG.WP.001 | strategist.sh: Permission denied | git pull снимает +x | post-merge hook, chmod +x |
| 2026-03-28 | ENG.WP.002 | cd: /IWE/DS-strategy: No such file | hardcoded путь IWE→ перенесено в Github | заменить IWE → $HOME, плейсхолдеры |
| 2026-03-29 | ENG.WP.004 | notify.sh не найден после day-plan | старый путь /IWE/DS-IT-systems/... в strategist.sh:64 | заменить на $HOME/Github/FMT-exocortex-template |
| 2026-03-30 | WP-29 | Artifact-gated atomicity | scheduler не проверял реальные артефакты | обновлён scheduler |
| 2026-04-04 | ENG.WP.005 | WARN: strategist morning failed каждый день | exit 2 (SKIP) интерпретируется как ошибка | scheduler различает exit 2 от exit ≠ 0 |

---

## Паттерны поломок (из ENG.WP.000)

1. **Path Drift** — hardcoded абсолютные пути `/IWE/...` устаревают при переносе. Встречался 3 раза.
   - Профилактика: `$HOME`, `$(dirname "$0")`, плейсхолдеры `{{WORKSPACE_DIR}}`
   - Диагностика: `grep -rn "IWE/DS-IT-systems" ~/Github/`

2. **+x права слетают** — `git pull` снимает execute-бит.
   - Фикс: post-merge hook в FMT-exocortex-template

3. **OAuth 401 без алерта** — агенты молча падают, никто не знает.
   - Фикс: health-check.sh + осталась задача: macOS osascript уведомление при 401

4. **Lock остаётся как директория** — strategist.sh создаёт lock через `mkdir`, при краше trap EXIT не срабатывает.
   - Известный инцидент: day-close.2026-03-22.lock провисел до 04.04
   - Нет системного фикса: нужен cron/cleanup старых locks

5. **SKIP ≠ FAILED** — exit 2 из strategist при lock воспринимается scheduler как ошибка.
   - Фикс применён 04.04 (d932681)

---

## Архитектура scheduler → strategist (как работает сейчас)

```
launchd (каждые 14 мин)
  → scheduler.sh dispatch
      → strategist.sh morning  (04:00–21:59, один раз в день)
          → acquire_lock("day-plan")  [mkdir lock, trap EXIT cleanup]
          → already_ran_today? [grep "Completed scenario: day-plan" в логе]
          → run_claude "day-plan"  [запускает claude с промптом]
          → notify_telegram
      → strategist.sh note-review  (22:00+, один раз в день)
      → extractor.sh inbox-check   (каждые 3ч, 07–23)
      → daily-report.sh            (один раз, после 06:00)
      → daily-telegram-report.sh   (один раз, после 08:00)
```

**Lock механизм:**
- `mkdir lockdir` → атомарно, если создался = мы владельцы
- `exit 2` при SKIP (lock exists = уже работает)
- `trap "rmdir lockdir" EXIT` → cleanup при завершении
- Проблема: если процесс убит (-9), trap не срабатывает → stale lock

**State файлы в `~/.local/state/exocortex/`:**
- `strategist-morning-YYYY-MM-DD` → создаётся через `mark_done`
- `extractor-inbox-check-last` → timestamp последнего запуска (interval check)
- `.status` файлы → детальный статус агентов (обновляются планировщиком)

---

## Текущие открытые проблемы (04.04.2026)

| # | Проблема | Приоритет | Статус |
|---|---------|-----------|--------|
| 1 | Stale lock cleanup — нет автоочистки старых lock-директорий | medium | open |
| 2 | OAuth 401 → macOS уведомление (по образцу Церена) | high | open |
| 3 | day-plan занимает 30+ мин — исследовать, оптимизировать? | low | open |
| 4 | week-review не отрабатывал ни разу (падает с exit 1) | medium | open |
| 5 | note-review-catchup никогда не успевал | low | open |

---

## Ключевые файлы

| Файл | Назначение |
|------|-----------|
| `FMT-exocortex-template/roles/synchronizer/scripts/scheduler.sh` | Главный диспетчер |
| `FMT-exocortex-template/roles/strategist/scripts/strategist.sh` | Стратег |
| `FMT-exocortex-template/roles/extractor/scripts/extractor.sh` | Экстрактор |
| `~/logs/synchronizer/scheduler-YYYY-MM-DD.log` | Лог планировщика |
| `~/logs/strategist/YYYY-MM-DD.log` | Лог стратега |
| `~/logs/strategist/locks/` | Lock-файлы (директории) |
| `~/.local/state/exocortex/` | State файлы (mark_done, mark_interval) |
| `~/.config/exocortex/telegram-token` | Telegram bot token |
| `~/.config/exocortex/telegram-chat-id` | Telegram chat ID (создан 04.04) |
| `DS-strategy/PACK-exocortex-engineering/04-work-products/ENG.WP.000` | Реестр инженерных WP |
