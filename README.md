# Digital Level Tradesman 4 Point — Web App

**Version 1.2** · 2026-07-19 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What changed in v1.2
Added a crosshair through the bubble's own center, reaching its own outer
edge (r:18) with no gap. Previously the bubble's crosshair stopped short
of its own edge, leaving a small gap that made it harder to precisely
match against the fixed background crosshair / center target. Now it's a
precise line-to-line match — same fix as Digital Level Tradesman
(standard) v1.9's bubble center line, in the 2D bull's-eye equivalent
(horizontal + vertical instead of just vertical).

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
