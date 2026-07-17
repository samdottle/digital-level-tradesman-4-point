# Digital Level Tradesman 4 Point — Web App

**Version 1.0** · 2026-07-16 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## What this is
Digital Level Pro 4 Point's bull's-eye + 4-point advisory UI and
leveling-screw workflow, running on **MPU-6050 instead of ADXL355** — a
"for now" stopgap while ADXL355 isn't in use for this tier, the same
situation Digital Level Pro itself went through before ADXL355 became
available again.

## Precision constants: kept identical to Pro 4 Point, on purpose
The ±0.01°/0.05°/0.10° threshold options, smoothing, and display scale are
unchanged from Pro 4 Point — not retuned for MPU-6050's noisier sensor.
This matches how Digital Level Tradesman (standard) is already a straight
rebrand of Pro's app rather than a redesign, and avoids inventing
unverified "better" numbers for a temporary product with no real hardware
data behind it yet.

**Known limitation, stated plainly:** the ±0.01° option is unlikely to
hold steady on MPU-6050 — its noise floor is high enough relative to that
threshold that the LEVEL label will likely flicker rather than settle,
regardless of display smoothing. This is a real sensor limitation, not a
bug. Worth field-testing alongside Tradesman itself before deciding
whether that option should be removed, relabeled, or left as-is for this
tier.

## Files
- `index.html` — the app. Full design rationale and the precision-tuning
  decision are documented in the header comment.
- `manifest.json`, `sw.js` — sw.js ships with `skipWaiting()`/`clients.claim()`
  from the start.
- `icon-192.png`, `icon-512.png` — carried over from Pro 4 Point unchanged.

## Firmware
Talks to the same MPU-6050 firmware already used by Digital Level
Tradesman and Digital Level RV (`DigitalLevel_Firmware_v2.5.ino`) — BLE
UUIDs and device name ("Level") unchanged, so no new firmware was needed.

## Inherited, unchanged from Pro 4 Point
Purely-relative advisory (no real geometry), Bubble Demo, Screw Demo, the
Tare double-counting fix, and the service worker update fix.

## Known gap
No `icons/` folder included — same as every other package in this family.
