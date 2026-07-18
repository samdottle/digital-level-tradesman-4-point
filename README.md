# Digital Level Tradesman 4 Point — Web App

**Version 1.1** · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## This is a clean restart from v1.0
A previous v1.1–v1.4 line was discarded in full — a bundled set of
changes made real problems worse and hard to isolate. This build starts
fresh from v1.0 with **exactly one change**, deliberately isolated so it
can be tested on its own before anything else is touched.

## The fix
Tapping Tare did not settle the bubble at center — it flashed to 0 for
one frame, then visibly slid back toward its old off-center position
(reported as "moves the bubble left every time Tare is tapped").

**Root cause:** Tare reset the displayed value (pitch/roll) to 0, but
never reset the EMA smoothing accumulator feeding it. That accumulator
kept holding its stale pre-tare value, so the very next real packet
blended the firmware's now-corrected ~0 reading into the OLD value —
e.g. `0.05×0 + 0.95×(-5) = -4.75` on the first packet alone — and it took
dozens of packets to fully converge back to 0.

**Fix:** reset the accumulator itself in the Tare handler, alongside the
existing display reset.

## Nothing else changed
No smoothing-rate changes, no UI changes, no other fixes bundled in —
confirmed by diff against v1.0, which shows exactly one added line. Please
test this in isolation before any further changes are requested.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
