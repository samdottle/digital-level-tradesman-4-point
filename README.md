# Digital Level Tradesman 4 Point — Web App

**Version 1.2** · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

Built on top of v1.1's isolated Tare fix — one additional isolated change.
(v1.1's fix is not yet independently confirmed working; this connect-time
issue may have been hit before Tare itself was tested.)

## The problem
With the unit genuinely level and flat, the bubble slams to maximum-left
the instant the app connects, before any Tare or other interaction.
Confirmed this is **not** a real-tilt display — the unit was level — so
this is a software issue, not the app correctly showing a real reading.

## Diagnosis (best available, not independently verified)
I don't have hardware access to confirm this directly against the
firmware. The best available explanation: the very first BLE packet after
connecting was applied completely raw and unsmoothed, with no validation
at all. If that first sample happens to be a transient/bad value — a
known real-world characteristic of some IMUs right as they start
streaming, before their own readings have settled — it went straight to
the display unfiltered.

## The fix
Buffer the first few packets (`STARTUP_SAMPLES = 3`) after connecting and
seed the display with their average, instead of trusting any single one
of them. Reset on disconnect too, so a reconnect gets its own fresh
settling window.

**This is a defensive fix, not a confirmed root cause.** If the
left-jump on connect persists after this, that rules out this theory and
points to something else — please report precisely if so, rather than
assume this was the fix.

## Nothing else changed
Confirmed by diff against v1.1 — the only other change is resetting the
new settling buffer on disconnect, which is required for the fix to work
correctly on a second connection.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
