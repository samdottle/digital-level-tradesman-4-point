# Digital Level Tradesman 4 Point — Web App

**Revision 2** (based on v1.0) · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What was reported against Rev 1
With the unit freshly tared offline, connect shows the bubble centered
correctly. Tapping Tare then makes it drift to -2.90° left. Disconnecting
and reconnecting shows the *same* -2.90° again. Tapping Tare a second
time does not move it at all — stays at -2.90°.

## Diagnosis
Traced this precisely against the firmware's actual `CAL:` (absolute set)
and `TAR:` (relative add) semantics: this exact sequence is what you'd
see if the **clear** step (`CAL:0.00,0.00`) succeeds every time, but the
follow-up **set** step (`CAL:<observed raw>,<observed raw>`) never takes
effect. Clear succeeding reveals the true, previously-masked raw tilt
(-2.90°); if set then silently fails, that raw tilt is what stays on
screen permanently, unaffected by reconnects or repeated attempts —
matching all four reported observations exactly.

## What changed in Rev 2
1. **Found a real, concrete gap:** `send()` had a completely silent
   `catch(_){}` — any BLE write failure produced zero indication, to the
   user or in the code. Now returns `true`/`false` for success/failure
   and surfaces a toast on any caught write error.
2. **Tare's clear step is now awaited**, and checked for success, before
   the sequence proceeds to collect raw samples — previously it was
   fire-and-forget with no guarantee it had actually completed before
   moving on, which could let the clear and set writes overlap/race.

## Honesty about confidence level
I traced the reported symptom to a specific, plausible mechanism and
fixed the two most likely contributing issues I could find in the code.
I cannot confirm from here whether either was the actual root cause, or
whether some other issue — e.g. the firmware itself failing to process
two commands sent close together, which no app-side fix can address — is
still present. **If -2.90° (or similar stuck-at-raw-value behavior)
persists after this**, that points away from both fixes in this revision
and toward the firmware's own command handling as the next place to look.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
