# Digital Level Tradesman 4 Point — Web App

**Version 1.3** · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What changed in v1.3
Removed the "MPU-6050 · ESP32 · BLE · vX.X" subtitle from the app header —
internal sensor/chip detail that doesn't belong in front of the end user,
matching the same fix already applied to Digital Level Tradesman
(standard) v1.6.

This was a genuine gap on my end, not just a stale-deployment issue:
earlier releases only bumped the version *number* inside that line — the
line itself was never actually removed until now. The footer's separate
"Arduino Nano ESP32 + MPU-6050 · BLE Nordic UART" credit is left as-is,
matching how Tradesman (standard) was handled — only the header subtitle
was removed there too, not the footer.

## Also confirmed still present (carried over correctly from v1.1/v1.2)
- Fixed center crosshair dot in the bull's-eye (v1.1)
- Adaptive two-rate bubble smoothing (v1.2)

If either of those still isn't showing up after deploying this version,
it's very likely a stale GitHub Pages / service worker cache issue rather
than a missing fix — the code for both is confirmed present in this file.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
