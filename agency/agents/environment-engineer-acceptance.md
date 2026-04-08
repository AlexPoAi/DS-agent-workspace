# Acceptance Runbook — Environment Engineer

## Назначение

Этот файл задаёт truthful acceptance-семантику для `Environment Engineer`.

Он нужен, чтобы отделять:
- подтверждённую diagnostic/fix способность;
- target-capability meta-verification слоя;
- реальные критерии `pass / partial / broken`.

## Подтверждённый scope на текущий момент

### Confirmed now

- root-cause диагностика operational сбоев по логам и runtime-артефактам;
- правка скриптов среды (`strategist`, `extractor`, `scheduler`, `close-task`, `daily-report`);
- truthful post-check после инженерных изменений;
- hardening status/report/runtime semantics;
- восстановление canonical routes и runtime contracts.

### Target capability, not yet proven

- повторяемый acceptance-service для всех остальных агентов по единой процедуре;
- полностью закрытый verification-loop по всем агентам без ручной инженерной интерпретации;
- формальная карта зрелости агентного слоя как отдельный сервисный контур.

## Verification scenarios

### 1. Runtime Diagnosis

**Что проверяем:**
- инженер умеет получить truthful runtime verdict без гадания;
- использует диагностический артефакт, а не предположение.

**Pass:**
- есть проверяемый diagnostic output;
- root cause или runtime-state подтверждены инструментально.

**Partial:**
- сигнал найден, но требует ручной сверки со смежными артефактами.

**Broken:**
- диагноз строится на предположении;
- инструментальный сигнал противоречит заявленному выводу.

### 2. Broken Runtime Fix

**Что проверяем:**
- инженер чинит именно root cause, а не только внешнее проявление;
- после фикса есть clean rerun/post-check.

**Pass:**
- найден root cause;
- внесён fix;
- post-check подтверждает, что проблема ушла.

**Partial:**
- проблема ослаблена, но не закрыта end-to-end.

**Broken:**
- обходной путь вместо root cause;
- нет post-check;
- success заявлен без доказуемого улучшения.

### 3. Meta-Verification Claim

**Что проверяем:**
- может ли `Environment Engineer` уже считаться полноценным verification-owner всего агентного слоя.

**Pass:**
- по нескольким агентам собраны repeatable acceptance verdict'ы с проверяемыми артефактами.

**Partial:**
- инженер уже умеет организовать truthful verification slices, но процесс пока остаётся во многом ручным.

**Broken:**
- claim заявлен, но воспроизводимого acceptance-loop нет.

> До отдельного повторяемого acceptance-cycle этот сценарий считать `target capability`.

## Truthful verdict rules

- `ready` допустим только там, где diagnostic/fix loop подтверждён живыми артефактами.
- `partial` означает: инженерный repair loop подтверждён, но meta-verification слой ещё не стал fully repeatable service.
- `broken` означает: root cause не доказан или fix не подтверждён post-check.
