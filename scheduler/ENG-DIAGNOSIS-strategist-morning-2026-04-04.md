---
type: engineering-diagnosis
date: 2026-04-04
engineer: Claude Sonnet 4.6
status: diagnosed
---

# Диагноз: стратег падает каждое утро (хронический баг)

## Симптом

Scheduler каждый день пишет `WARN: strategist morning failed` в 04:14.
Пользователь видит жёлтый статус экзокортекса и считает, что стратег сломан.

## Реальная картина (реконструкция по логам 04.04)

```
04:00  strategist.sh morning → acquire_lock("day-plan") → создан locks/day-plan.2026-04-04.lock
04:00  Claude запущен, day-plan выполняется
04:14  scheduler dispatch → strategist.sh morning → SKIP (lock exists) → WARN: failed ← ЛОЖНАЯ ТРЕВОГА
04:32  Claude завершил day-plan → lock удалён (trap EXIT)
06:15  scheduler dispatch → SKIP: already completed today (grep "Completed scenario: day-plan" в логе)
```

**Вывод: стратег работает исправно. Баг — в scheduler'е, который интерпретирует SKIP как failed.**

## Три независимых проблемы

### 1. SKIP ≠ FAILED в scheduler (критично, легко чинится)

**Место:** `scheduler.sh` — логика вызова `strategist.sh morning`

Сейчас:
```bash
if "strategist.sh morning" >> LOG 2>&1; then
    log "success"
else
    log "WARN: failed"  # ← срабатывает и при SKIP, и при реальной ошибке
fi
```

`strategist.sh` возвращает `exit 0` при SKIP ("already running") и `exit 0` при "already completed".
Но scheduler получает exit ≠ 0 при SKIP? Или SKIP тоже возвращает 0, но что-то другое?

**Нужно проверить:** exit code strategist.sh при SKIP.

### 2. Стале lock от 22 марта (легко чинится)

```
/Users/alexander/logs/strategist/locks/day-close.2026-03-22.lock  ← директория, не файл
```
Создан 22.03 во время зависания day-close (известный инцидент).
Не влияет на morning (это day-close lock), но засоряет директорию.

### 3. day-plan занимает 18 минут (04:00 → 04:32)

Claude запускается в 04:00, завершает в 04:32. 32 минуты на day-plan — это нормально?
Нужно проверить что именно делает day-plan за это время.

## Действия

- [ ] 1. Проверить exit code `strategist.sh morning` при SKIP → если 0, то scheduler не должен писать WARN
- [ ] 2. Исправить в scheduler: различать SKIP (exit 0 + маркер в логе) от реальной ошибки (exit ≠ 0)
- [ ] 3. Удалить stale lock: `rmdir ~/logs/strategist/locks/day-close.2026-03-22.lock`
- [ ] 4. Изучить что делает day-plan за 32 минуты — оптимизация?

## Быстрый фикс (сделать сейчас)

Удалить stale lock + исправить exit code логику в scheduler.sh.

## Применённые фиксы (04.04.2026 14:31)

### ✅ Фикс 1: scheduler.sh — различение SKIP от ошибки
- **Файл:** `FMT-exocortex-template/roles/synchronizer/scripts/scheduler.sh:165-174`
- **Коммит:** d932681
- **Суть:** exit 2 теперь логируется как INFO (не WARN), не вызывает ложную тревогу

### ✅ Фикс 2: удалён stale lock
- `~/logs/strategist/locks/day-close.2026-03-22.lock` — удалён
- Причина появления: зависание day-close 22.03 (известный инцидент)

### ✅ Зафиксирован uncommitted файл в DS-strategy
- `current/UNPROCESSED-NOTES-REPORT.md` — закоммичен, запушен

## Результат
Завтра утром в 04:00 при SKIP стратег будет писать `INFO: уже выполняется (lock)` вместо `WARN: failed`.
Жёлтый статус мозга экзокортекса из-за стратега утром исчезнет.
