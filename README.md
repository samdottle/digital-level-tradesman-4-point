# Digital Level Tradesman 4 Point — Web App

**Version 1.2** · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What changed in v1.2
The bubble felt very slow to respond while actually connected to hardware
(confirmed snappy in Bubble Demo, which bypasses the sensor/smoothing path
entirely — that comparison is what isolated the cause). Root cause: fixed
`EMA_ALPHA = 0.05` smoothing alone. The math: reaching 95% of a real step
change takes ~58 samples, 99% takes ~90 — several seconds at a typical
streaming rate. Purely a smoothing artifact, not a hardware or noise
problem.

**Fix: adaptive two-rate smoothing**, applied independently per pitch and
roll (a real adjustment often only changes one axis at a time — e.g.
turning one screw). While a new sample differs from the current smoothed
value by more than `ADAPT_THRESHOLD_DEG` (0.15°), the display uses a fast
alpha (0.30) to catch up quickly; once close to settled, it drops back to
the original 0.05 for the same low-jitter stability as before. This does
**not** change steady-state accuracy — a settled EMA converges to the same
mean regardless of alpha — it only changes how long it takes to get there.

`ADAPT_THRESHOLD_DEG` is a reasoned starting estimate (set above
MPU-6050's expected per-sample noise, below a typical real screw-turn
change), not a value verified against real MPU-6050 noise data yet. Worth
field-tuning once tested — and this same technique may be worth applying
to Digital Level Tradesman (standard), which shares the same sensor.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
