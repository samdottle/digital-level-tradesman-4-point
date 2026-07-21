# Digital Level Tradesman 4 Point — Web App

**Version 1.5** · 2026-07-21 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What changed in v1.5
Added a 2-position in-app Calibration feature. Previously **absent from
this product entirely** — only Tare existed in the app; Calibration was
standalone-button-only. This gap was flagged, then deferred, when the
original Pro 4 Point + Tradesman 4 Point Operator's Guide was built (that
guide's Calibration section described a feature that didn't actually
exist in either app). Ported from Digital Level Tradesman (standard),
which already has this feature working correctly.

Sampling reads directly from `epRef.current`/`erRef.current` (the same
smoothed accumulator that feeds the display), rather than adding a
separate pitch/roll-tracking ref pair — one less piece of state to keep
in sync with the existing Tare/EMA machinery.

## Critical fix applied deliberately, not overlooked
This product negates roll on receipt for display
(`rr=-parseFloat(m[2])`), same as it always has. The calibration set
command negates the sampled roll average back (`-or_.toFixed(2)`) before
sending, exactly matching the fix already applied to this product's Tare
in v1.4 — sending the already-negated display value directly would
reproduce the exact bug found and fixed there: the error doesn't
correct, it doubles with each use. Pitch needs no such correction; it
was never negated.

## Confirmed by diff against v1.4
Every changed line is an *addition* — nothing existing was removed or
modified. This is a purely additive feature, isolated from Tare, the
bull's-eye display, demo modes, and BLE connect/disconnect handling.

## Pro 4 Point intentionally not touched
Set aside for a future update, per explicit instruction, to keep this
change scoped and shippable today.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, FRONT/REAR bubble-mimic UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — **still needs this same Calibration feature ported**

## Known gap
No `icons/` folder included — same as every other package in this family.
