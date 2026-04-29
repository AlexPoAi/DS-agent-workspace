# Скилл: finance-view-builder

> **Агент:** vk-finance-analyst
> **Назначение:** Построить Finance View по точкам VK Coffee из нормализованных данных

---

## Вход

Принимает `ingestion_result` из скилла `drive-sales-ingestion.md`:
```python
{
    'status': 'done' | 'partial',
    'period_start': 'YYYY-MM-DD',
    'period_end': 'YYYY-MM-DD',
    'daily_data': [...],  # нормализованные строки по дням и точкам
    'missing': [],
}
```

---

## Расчёты

### Метрики по каждой точке

```python
from collections import defaultdict
from datetime import datetime

def build_finance_view(ingestion_result):
    data = ingestion_result['daily_data']
    period_start = ingestion_result['period_start']
    period_end = ingestion_result['period_end']
    
    # Число дней в периоде
    days = (datetime.strptime(period_end, '%Y-%m-%d') - 
            datetime.strptime(period_start, '%Y-%m-%d')).days + 1
    
    # Агрегация по точке
    by_point = defaultdict(lambda: {'revenue': 0, 'cost': 0, 'days_with_data': set()})
    
    for row in data:
        pt = row['point']
        by_point[pt]['revenue'] += row.get('revenue', 0)
        by_point[pt]['cost'] += row.get('cost', 0)
        by_point[pt]['days_with_data'].add(row['date'])
    
    point_metrics = {}
    for pt, agg in by_point.items():
        revenue = agg['revenue']
        cost = agg['cost']
        days_actual = len(agg['days_with_data'])
        margin_pct = round((revenue - cost) / revenue * 100, 1) if revenue > 0 else None
        avg_per_day = round(revenue / days, 0) if days > 0 else None
        
        point_metrics[pt] = {
            'revenue': revenue,
            'avg_per_day': avg_per_day,
            'margin_pct': margin_pct,
            'days_actual': days_actual,
        }
    
    return point_metrics, days
```

### Тренд (сравнение с предыдущим периодом)

```python
def calculate_trend(current_revenue, prev_revenue):
    """↑ рост >5%, ↓ падение >5%, = стабильность ±5%"""
    if prev_revenue is None or prev_revenue == 0:
        return '?'
    delta = (current_revenue - prev_revenue) / prev_revenue * 100
    if delta > 5:
        return '↑'
    elif delta < -5:
        return '↓'
    else:
        return '='
```

Если данных предыдущего периода нет — тренд = `—`.

### Выявление аномалий

```python
def detect_anomalies(daily_data, threshold_pct=20):
    """
    Аномалия: день, когда выручка точки отклоняется от её средней >20%.
    """
    by_point_days = defaultdict(list)
    for row in daily_data:
        by_point_days[row['point']].append((row['date'], row.get('revenue', 0)))
    
    anomalies = []
    for pt, days_data in by_point_days.items():
        revenues = [r for _, r in days_data]
        if len(revenues) < 3:
            continue
        avg = sum(revenues) / len(revenues)
        for date, rev in days_data:
            if avg > 0 and abs(rev - avg) / avg * 100 > threshold_pct:
                direction = 'просадка' if rev < avg else 'выброс'
                pct = round(abs(rev - avg) / avg * 100)
                anomalies.append({
                    'date': date,
                    'point': pt,
                    'revenue': rev,
                    'avg': round(avg),
                    'deviation_pct': pct,
                    'direction': direction,
                })
    
    return sorted(anomalies, key=lambda x: x['deviation_pct'], reverse=True)
```

---

## Порядок точек в Finance View

Канонический порядок (по убыванию выручки, но всегда показывать все три):
1. turgeneva (Тургенева 20)
2. lugovaya (Карьерный переулок)
3. samokisha (Самокиша 5Б)

Русские названия в отчёте:
```python
POINT_DISPLAY = {
    'turgeneva': 'Тургенева 20',
    'lugovaya': 'Карьерный пер.',
    'samokisha': 'Самокиша 5Б',
}
```

---

## Формирование текста Finance View

```python
def format_finance_view(point_metrics, anomalies, ingestion_result, prev_metrics=None):
    period_start = ingestion_result['period_start']
    period_end = ingestion_result['period_end']
    today = datetime.now().strftime('%Y-%m-%d')
    status = ingestion_result['status']
    
    # Итого по сети
    total_revenue = sum(m['revenue'] for m in point_metrics.values())
    total_days = list(point_metrics.values())[0].get('days_actual', 1) if point_metrics else 1
    avg_per_day_network = round(total_revenue / total_days) if total_days > 0 else 0
    
    # Взвешенная маржа по сети (упрощённо — среднее по точкам с данными)
    margins = [m['margin_pct'] for m in point_metrics.values() if m['margin_pct'] is not None]
    avg_margin = round(sum(margins) / len(margins), 1) if margins else None
    
    lines = [
        f"# Finance View — VK Coffee",
        f"Период: {period_start} — {period_end}",
        f"Сгенерировано: {today}",
        f"Агент: vk-finance-analyst",
        "",
        "## Сводка по сети",
        f"- Выручка итого: {total_revenue:,.0f} ₽".replace(',', ' '),
        f"- Средняя/день: {avg_per_day_network:,.0f} ₽".replace(',', ' '),
        f"- Маржа: {avg_margin}%" if avg_margin else "- Маржа: нет данных",
        "",
        "## По точкам",
        "| Точка | Выручка | Ср/день | Маржа | Тренд |",
        "|-------|---------|---------|-------|-------|",
    ]
    
    for slug in ['turgeneva', 'lugovaya', 'samokisha']:
        display = POINT_DISPLAY.get(slug, slug)
        if slug in point_metrics:
            m = point_metrics[slug]
            prev_rev = prev_metrics.get(slug, {}).get('revenue') if prev_metrics else None
            trend = calculate_trend(m['revenue'], prev_rev)
            margin_str = f"{m['margin_pct']}%" if m['margin_pct'] is not None else "нет данных"
            lines.append(
                f"| {display} | {m['revenue']:,.0f} ₽ | {m['avg_per_day']:,.0f} ₽ | {margin_str} | {trend} |"
                .replace(',', ' ')
            )
        else:
            lines.append(f"| {display} | — | — | — | — |")
    
    lines.append("")
    lines.append("## Аномалии")
    if anomalies:
        for a in anomalies[:5]:  # не больше 5 аномалий
            pt_display = POINT_DISPLAY.get(a['point'], a['point'])
            lines.append(
                f"- {a['date']} {pt_display}: {a['direction']} -{a['deviation_pct']}% "
                f"(факт {a['revenue']:,.0f} ₽ vs средн. {a['avg']:,.0f} ₽)".replace(',', ' ')
            )
    else:
        lines.append("- Аномалий не выявлено")
    
    lines.extend([
        "",
        "## Для financial-consultant",
    ])
    
    # Лидер по марже
    if margins:
        leader_slug = max(point_metrics, key=lambda s: point_metrics[s].get('margin_pct') or 0)
        lines.append(f"- Лидер по марже: {POINT_DISPLAY.get(leader_slug, leader_slug)} ({point_metrics[leader_slug]['margin_pct']}%)")
    
    # Отстающий по выручке
    if point_metrics:
        laggard_slug = min(point_metrics, key=lambda s: point_metrics[s]['revenue'])
        lines.append(f"- Наименьшая выручка: {POINT_DISPLAY.get(laggard_slug, laggard_slug)}")
    
    lines.extend([
        "",
        "---",
        f"Агент: VK Finance Analyst",
        f"Дата: {today}",
        f"Статус: {status}",
    ])
    
    if ingestion_result.get('missing'):
        lines.append(f"Partial: {', '.join(ingestion_result['missing'])}")
    
    return '\n'.join(lines)
```

---

## Сохранение

```python
import os

def save_finance_view(content, period_end):
    views_dir = '/Users/alexander/Github/DS-finance-private/business-finance/views'
    os.makedirs(views_dir, exist_ok=True)
    filename = f"finance-view-{period_end}.md"
    filepath = os.path.join(views_dir, filename)
    with open(filepath, 'w', encoding='utf-8') as f:
        f.write(content)
    return filepath
```

**Важно:** папка `DS-finance-private/` — локальная, не пушить в git.

---

## Выход скилла

```python
view_result = {
    'filepath': 'DS-finance-private/business-finance/views/finance-view-YYYY-MM-DD.md',
    'status': 'done' | 'partial',
    'period': 'YYYY-MM-DD — YYYY-MM-DD',
    'total_revenue': 2347315,
    'points': {
        'turgeneva': {'revenue': 1167567, 'margin_pct': 61.8},
        'lugovaya':  {'revenue':  640338, 'margin_pct': 61.4},
        'samokisha': {'revenue':  539410, 'margin_pct': 73.3},
    },
    'anomalies_count': 2,
    'ready_for_verdict': True,  # True если status == 'done'
}
```

Передать `view_result` пользователю и в financial-consultant для verdict.
