# Digital Level Tradesman 4 Point — Web App

**Version 1.6** · 2026-07-21 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What was reported against v1.5
A precise, reproducible test: power on, connect, Tare to zero display,
then Calibrate. First calibration left the bubble 2.35° off. Tapping
Re-Calibrate moved it back to center.

## Root cause, verified numerically before writing any fix
The 2-position average v1.5 computed is `(O − B)` for roll or `(B − O)`
for pitch, where `O` is whatever offset was **already in effect** when
sampling began — not necessarily 0. v1.5's math assumed `O=0`
unconditionally, so any pre-existing offset (from a prior Tare, exactly
as tested) leaked directly into the result. The reported **-2.35°** was
reproduced *exactly* by this mechanism in simulation before any fix was
written. Re-Calibrate improved it a lot but didn't zero it exactly,
since the identical flawed math was simply reapplied a second time.

## The fix
Track the offset the app itself knows is currently in effect, and solve
for the pure hardware bias directly instead of assuming a zero starting
point. `CAL:` is confirmed (from firmware source) to be an absolute set,
so the app can be certain of the resulting offset immediately after any
successful `CAL:` command, regardless of what it was before. Verified
correct for both axes independently via simulation (roll: with
negation; pitch: without) before implementing.

## Honest, known limitation — not hidden
At connect time, this session has no way to know what offset the
firmware already has persisted from a prior session or
standalone/physical-button use — there's no query command in this
protocol to ask. So the **first** calibration in a fresh session may
still land off by whatever that unknown prior offset was, same as v1.5.

**Every calibration after that first one is now mathematically exact**,
not just visually close, because by then the tracked offset is
genuinely known. If a first calibration looks off, tapping Re-Calibrate
once now resolves it *exactly*, not just approximately.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, FRONT/REAR bubble-mimic UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — still needs Calibration ported, and would need this same fix applied

## Known gap
No `icons/` folder included — same as every other package in this family.
