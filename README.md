# Digital Level Tradesman 4 Point — Web App

**Revision 7** (based on v1.0) · 2026-07-19 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## A real, verified bug — not another timing theory
A separate AI (ChatGPT) reviewed the handoff document and, instead of
proposing another timing experiment, questioned whether the receive-side
coordinate negation might be inconsistently applied between the display
path and the command-construction path. This was checked rigorously —
worked out algebraically and verified numerically against the firmware's
actual transmit formula and `TAR:` handler — and confirmed as a real,
previously undiscovered bug, independent of everything tried in
Revisions 1–6.

## The bug
This product negates roll on receipt for display purposes
(`rr=-parseFloat(m[2])`) — confirmed, by direct comparison, that
Tradesman standard has **no such negation**. Revisions 1–6 all sent that
already-negated value straight into `TAR:`/`CAL:` commands, which the
firmware interprets in *its own*, non-negated convention.

Worked out precisely: this doesn't just fail to correct the reading — it
computes `rollOffset_new = 2×rollOffset − roll` instead of the correct
`rollOffset_new = roll`. The error doesn't shrink, it **doubles with each
tap**. This matches the very first symptom ever reported in this entire
investigation: "moving the bubble left each time Tare is tapped."

## Family-wide implication
Confirmed via source comparison that **Pro 4 Point has this exact same
unguarded negation** and sends `TAR:` the same way — very likely the same
bug, not fixed there yet. Same pattern as the EMA-accumulator bug found
earlier in this investigation (also present in all four sibling products,
only fixed in this one so far).

## Rev 8 — Precision Crosshair

Rev 8 is based directly on the verified working Rev 7 Gold Standard. It makes one UI-only change: the bull's-eye now has a compact white crosshair and center point drawn above the moving bubble, keeping the exact target visible during fine adjustment. No BLE, tare, smoothing, calibration, or firmware-facing logic was changed.

## What changed in Rev 7
Roll is now negated back — undoing the display-side negation —
specifically when constructing the `TAR:` command's roll value, so the
firmware receives its own convention instead of the app's display
convention. Pitch is unaffected; it was never negated.

## Why this one is different from Revisions 1–6
This is the first fix in the entire investigation backed by a worked,
numerically-verified derivation showing the *old* code produces a
specific wrong number, and the *new* code produces the exact right one —
rather than a plausible-sounding theory tested by trial and error.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — **has the same unfixed bug described above**

## Known gap
No `icons/` folder included — same as every other package in this family.
