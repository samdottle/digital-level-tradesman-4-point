# Digital Level Tradesman 4 Point — Web App

**Proto 1.0 — locked down for shipment**
2026-07-21 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

Built from v1.7 (Calibration fixed correctly via TAR:, per a separate
AI's structural finding).

## What changed in this lockdown pass
Two version-visibility fixes, both genuinely overdue — the equivalent
fix had already been made on Digital Level Tradesman (standard) but was
never carried over here:

1. **Added a version subtitle under the header title** (previously
   absent entirely) — 12px, `#94a3b8`, chosen for contrast against the
   dark header background.
2. **Improved footer readability** — increased from 11px/`#1e3a5f` (a
   dark navy, low-contrast against the dark background) to 13px/`#64748b`,
   a lighter, more visible gray. This addresses the actual readability
   complaint, not just adds a second copy in the same hard-to-read style.

No functional changes in this pass.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, FRONT/REAR bubble-mimic UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — still needs Calibration ported (v1.7's TAR:-based approach)

## Known gap
No `icons/` folder included — same as every other package in this family.
