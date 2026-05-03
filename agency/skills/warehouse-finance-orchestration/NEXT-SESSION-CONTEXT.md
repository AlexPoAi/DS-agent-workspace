---
type: session-context
created: 2026-05-03
status: ready-to-execute
priority: critical
---

# Контекст следующей сессии: обработка отчётов Жанны

## Команда для старта

```
продолжаем дальше
```

Агент читает этот файл и сразу знает что делать.

---

## Что уже готово (архитектура зафиксирована)

| Документ | Что содержит |
|----------|-------------|
| `orchestration-protocol.md` | 4-этапный протокол: INTAKE → Кладовщик → Экономист → Архив |
| `IO-CONTRACT.md` | Полный контракт входов/выходов обоих агентов |
| `agency/agents/warehouse-demand-analyst.md` | Агент кладовщика (обновлён, входы/выходы встроены) |
| `agency/agents/vk-finance-analyst.md` | Агент экономиста (обновлён, 8 секций Finance View) |
| `PACK-warehouse/03-methods/WH.METHOD.001-DOMAIN-SCOPE` | Что читает кладовщик |
| `agency/skills/vk-finance-analyst/DOMAIN-SCOPE` | Что читает экономист |

---

## Что нужно сделать в следующей сессии

### Приоритет 1 — ОБРАБОТАТЬ ДАННЫЕ (сегодня обязательно)

Файлы уже скачаны локально. Нужно запустить обоих агентов:

**Входные данные:**
```
/Users/alexander/Github/VK-offee/knowledge-base/Отчёты для бота/Новые документы/
```
Период: 15-30 апреля 2026

**Шаг 1: Кладовщик**
```bash
cd ~/Github/VK-offee
bash PACK-warehouse/tools/warehouse_full_loop.sh
```
Результат → PACK-warehouse/04-work-products/ (WH.SESSION.001, WH.CARD.*, WH.REPORT.002)

**Шаг 2: Экономист**
Запустить vk-finance-analyst на те же локальные файлы.
Результат → DS-finance-private/business-finance/views/finance-view-2026-04-15_2026-04-30.md

**Шаг 3: Архив**
Экономист перемещает ВСЕ файлы на Drive: "Новые документы" → "Обработано"

---

### Приоритет 2 — Посмотреть цифры экономиста

Цель пользователя: **понять финансовую картину за период 15-30 апреля**
- Выручка по точкам (turgeneva, lugovaya, samokisha)
- Маржа фактическая (из накладных, actual_margin)
- Аномалии и тренд
- Сравнение с предыдущим периодом (30.03–14.04 = 2,347,315 ₽, 146,707 ₽/день)

---

## Ключевые факты которые агент должен держать в уме

| Факт | Значение |
|------|---------|
| Папка входных данных | `VK-offee/knowledge-base/Отчёты для бота/Новые документы/` |
| Папка Drive "Новые документы" ID | `1Jg1Zgj2_ueTV6-NQamAU8XCPalFNhO8W` |
| Папка Drive "Обработано" ID | `1WanukzWeuqJgUQ7N8YkG9oC23-DIRGjT` |
| Кладовщик пишет в | `VK-offee/PACK-warehouse/` |
| Экономист пишет в | `DS-finance-private/business-finance/` (LOCAL ONLY, не git) |
| Кто двигает файлы | ТОЛЬКО экономист, после обработки обоих |
| Маржа кладовщика | catalog_margin (из Каталога) |
| Маржа экономиста | actual_margin (из Накладных, COGS) |
| Point slugs | turgeneva / lugovaya / samokisha |
| Предыдущий период | 30.03–14.04: 2,347,315 ₽ выручка, 146,707 ₽/день |

---

## Что ещё pending (не срочно, но зафиксировать)

- Скрипт оркестратора `warehouse-biweekly-processor.sh` — заглушка, нужно переписать под 4 этапа
- Python class mismatch: `SabyGoogleDriveParser` vs `GoogleDriveParser` в finance skills → ImportError при запуске
- Canonical `drive-folders-vkoffee.yaml` — создан, folder ID нужно верифицировать

