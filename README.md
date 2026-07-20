# Digital Level Tradesman 4 Point — Web App

**Version 1.4** · 2026-07-19 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What changed in v1.4
Bubble response speed. Built clean on top of v1.3's corrected baseline
(branding removal preserved, confirmed below), rather than carried
forward from the killed v1.3 build.

Re-added adaptive two-rate EMA smoothing:
- Fast alpha (0.30) while a reading is still catching up to a real
  change (e.g. someone turning a leveling screw)
- Drops back to the original slow, stable alpha (0.05) once close to
  settled
- Applied independently per axis, since a real adjustment often only
  changes one
- Does **not** change steady-state accuracy — only how long it takes to
  get there

## Confirmed before implementing
- Isolated from Tare, connect-time startup averaging, and all three demo
  modes — same verification as the killed build; none of those touch
  this smoothing-rate logic
- "MPU-6050 · ESP32 · BLE" branding remains removed from both the header
  and footer (v1.3's fix) — this version only adds the smoothing change,
  nothing else

## Still known, not yet done
- Version number isn't visible under the product name at the top (only
  in the footer) — the same fix Tradesman standard got in v1.8, not yet
  applied here
- Pro 4 Point still has the branding-removal gap v1.3 fixed here, not
  yet ported

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
