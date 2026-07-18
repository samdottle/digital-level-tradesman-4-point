# Digital Level Tradesman 4 Point — Web App

**Revision 1** (based on v1.0) · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## Why "Revision" instead of "Version"
Switching to Rev N labeling for this ongoing Tare troubleshooting effort,
at your request — the discarded v1.1–v1.4 line, plus the two further
v1.1/v1.2 iterations built after that restart, made version numbers
themselves a source of confusion. Revisions are **not** confirmed
releases. Once the Tare problem is fully resolved, the final working
revision will be consolidated into a proper Version 1.1.

**Confined to Digital Level Tradesman 4 Point only** — no other product
in the family touched by this work yet.

## What changed in Rev 1: redesigned Tare
Previously, Tare sent `TAR:<displayed pitch>,<displayed roll>` — the
firmware **adds** this to whatever offset it already has stored
(confirmed directly against the firmware source: `pitchOffset +=`).
That's the mathematically necessary approach given that the app only
ever sees the already-corrected reading, never the sensor's true raw
value — but it means any past display error, from any cause, got
permanently added into the firmware's stored (flash-persisted) offset,
with no way to recover except tapping Tare again from a since-corrected
reading.

**Fix:** Tare now clears the firmware's offset first (`CAL:0.00,0.00`,
which the firmware **sets** rather than adds — also confirmed against
the firmware source), waits for a few clean raw samples once that offset
is wiped, then sets the offset directly from that observation
(`CAL:`, absolute) instead of adding to anything. This can't compound a
bad prior offset, whatever caused it — nothing from before the clear
factors into the new value.

## Three safeguards around the new async sequence
None of this can be verified against real BLE timing from here, so:
1. **Timeout** (3000ms) — if no raw packet arrives after clearing, abort
   back to idle instead of leaving Tare permanently stuck.
2. **Double-tap guard** — tapping Tare again mid-sequence is ignored
   (with a toast), rather than starting an overlapping second sequence.
3. **Disconnect guard** — disconnecting mid-sequence resets the tare
   state, so a future reconnect can't inherit stale mid-tare state.

## Honesty about confidence level
The underlying math (clear-then-set beats add-to-whatever's-already-there)
is verified correct on paper and against the actual firmware source code.
The async/timing mechanics of the three safeguards above are **not**
verified against real hardware — this needs real-device testing before
being trusted as fully working.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
