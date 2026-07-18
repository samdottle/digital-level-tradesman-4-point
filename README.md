# Digital Level Tradesman 4 Point — Web App

**Revision 3** (based on v1.0) · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## New evidence that shaped this revision
DL Tradesman (standard) does **not** show this problem — both pitch and
roll zero correctly there. That product sends only one command per tare
(`TAR:`, additive); this product's redesign sends two (`CAL:0.00,0.00`
clear, then `CAL:<value>` set). Same firmware in both cases, so this
specifically implicates sending two commands close together in time, not
command parsing being broken in general — and matches the earlier finding
that the observed -2.90° was clean and consistent (meaning the clear had
already fully taken effect by the time all raw samples were collected;
it's specifically the second write that isn't landing).

## What changed in Rev 3
Added a real wall-clock delay (300ms) between the clear write succeeding
and the app starting to trust subsequent packets as clean raw data. Rev
2's `await` only confirmed the *browser* handed the first write to the
BLE stack — it says nothing about whether the firmware's own processing
loop has actually finished acting on it before a second write arrives.
This delay is independent of packet rate, unlike relying on "however long
3 packets happen to take."

## Honesty about confidence level
300ms is a reasoned guess, not a measured value — I don't have visibility
into the firmware's actual loop timing.
- If this resolves it, great.
- If it helps but doesn't fully resolve it, the delay may just need to be
  longer, which would still support the same diagnosis.
- If it doesn't help at all, that undercuts the "commands too close
  together" theory and points somewhere else entirely.

Worth reporting precisely which of these happens.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
