# Digital Level Tradesman 4 Point — Web App

**Version 1.3** · 2026-07-19 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## Note on version history
An earlier v1.3 (adaptive-smoothing bubble-response fix) was killed
before shipping, because it was built on top of v1.2 without first
catching the bug described below — fixing v1.2's gap took priority.
**This v1.3 is a clean rebuild from v1.2 containing only this fix.** The
adaptive-smoothing work is set aside for a future version, to be redone
on top of this corrected baseline rather than carried forward as-is.

## What changed in v1.3
Removed "MPU-6050 · ESP32 · BLE" chip/sensor branding from both places it
was still showing — the header subtitle under the product name, and a
separate line in the footer.

This removal was already done for Digital Level Tradesman (standard)
back in v1.6 (header) and v1.8 (footer), but this product's own removal
(originally done in the old, discarded v1.1–v1.4 line) was lost when that
line was discarded and never re-applied during the clean rebuild that
followed. Caught and fixed now, on the declared threshold standard,
before shipping.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — **likely has this same unfixed gap, not checked yet**

## Known gap
No `icons/` folder included — same as every other package in this family.
