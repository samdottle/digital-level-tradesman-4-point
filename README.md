# Digital Level Tradesman 4 Point — Web App

**Version 1.8** · 2026-07-23 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

Built on top of the locked Proto 1.0. Not yet re-locked — that's a
separate, deliberate step once this fix is confirmed on real hardware.

## What was reported against Proto 1.0
"After a precise calibrate, the bubble does not completely center."

## Root cause, traced not guessed
Not a math error — v1.7's calibration fix (sending the correction via
`TAR:` instead of `CAL:`) remains correct and is unchanged here. The
actual gap: after Calibration sends its corrected offset, nothing resets
`epRef.current`/`erRef.current`, the EMA smoothing accumulators. They
keep holding their **pre-calibration** values, so the display only
gradually converges to the new, correct reading as each new packet
blends in at `EMA_ALPHA=0.05` — roughly 3–6 seconds to become visually
indistinguishable from centered, depending on telemetry rate. Someone
glancing at the display right after calibrating would see exactly what
was reported.

This is the **same bug class already found and fixed for Tare**, back in
v1.1 ("the display drifts back off-center after tare") — it just never
got ported to Calibration when that feature was built later, in v1.5.

## The fix — deliberately not a copy-paste of Tare's fix
Tare resets straight to 0, which is correct *there* because zeroing the
current position is the entire point of tare. Calibration corrects
sensor bias, not surface levelness — the reading right after calibrating
isn't necessarily 0, only if the calibration surface itself happened to
be level. Forcing a 0 reset here would be actively wrong on a non-flat
surface.

Instead, this reuses the existing, already-proven startup-buffer
mechanism (the same one already used at connect and disconnect): clear
the accumulators to `null` and let the display re-seed itself from a
handful of fresh, real post-calibration samples.

Applied in **both** places that change the firmware's stored offset:
`startPosition2` (the calibration result) and `resetCalibration` (the
Reset button) — Reset has the identical exposure, since it also changes
the offset without resetting the accumulator.

## Confirmed by diff against the shipped Proto 1.0
Every changed line is an *addition* — nothing existing was removed or
modified.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, FRONT/REAR bubble-mimic UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — still needs Calibration ported, and would need this same accumulator-reset fix applied too

## Known gap
No `icons/` folder included — same as every other package in this family.
