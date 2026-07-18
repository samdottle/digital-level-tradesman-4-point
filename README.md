# Digital Level Tradesman 4 Point — Web App

**Revision 5** (based on v1.0) · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What was reported against Rev 4
On tap, the bubble drifts *slowly* left (during the 3-second clear/delay
window), then *snaps* to center, then drifts *slowly* left again, back to
the same value as before.

## Diagnosis
Traced this precisely. Phases 1 and 3 are both honest reflections of real
incoming packets under normal EMA smoothing — nothing wrong with either.
**Phase 2 (the snap to center) was never confirmed** — the app sent the
`CAL:` set command and immediately assumed success, resetting the display
to 0 and showing "Tared" without checking whether the offset had actually
changed. If the set never took effect, phase 3 is simply the display
catching back up to the real, still-uncorrected value the firmware keeps
sending. The app was never lying about the physical reading — it was
making an *unconfirmed claim of success* in between.

## What changed in Rev 5
- Replaced the optimistic "send set command, assume success" step with a
  genuine confirmation step: after sending `CAL:<value>`, the app now
  watches subsequent real packets for evidence the reading has actually
  gone near zero (within 0.20°) before declaring success.
- If confirmation doesn't arrive within 2.5 seconds, the same computed
  value is resent once — covers a one-off dropped write rather than a
  deterministic failure.
- If the retry also isn't confirmed, Tare now reports an **honest
  failure** ("offset change not confirmed by the unit") instead of a
  false success that silently reverts a few seconds later.

## What this does and doesn't tell us
This does not by itself explain *why* the set fails. What it does is make
the app's reporting truthful instead of optimistic, and turns "does the
set actually work" into a directly observable yes/no — a real
"Tared — confirmed" vs. a real failure toast — rather than something that
had to be inferred by watching the bubble drift back over several
seconds.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
