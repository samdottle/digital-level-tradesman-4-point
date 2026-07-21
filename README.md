# Digital Level Tradesman 4 Point — Web App

**Version 1.7** · 2026-07-21 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## Fixes v1.5 AND v1.6's calibration bug correctly and completely
Replaces v1.6's approach rather than patching it further.

**v1.6's fix** (app-side offset tracking) turned out to only work
starting from a session's *second* calibration — confirmed by tracing
the actual shipped v1.6 code against the exact reported test sequence
(power on, connect, Tare, Calibrate): the tracking refs are still `null`
at that point, so the first calibration used the identical formula as
v1.5, producing "no visible change at all" exactly as reported. This was
a real, acknowledged gap in v1.6's scope, not a new or different bug.

## Root structural issue, found by ChatGPT
Reviewing a handoff document of this problem: both v1.5 and v1.6 sent
the calibration result via `CAL:`, which **replaces** the firmware's
stored offset absolutely — but the 2-position average was never a
complete replacement value, only a **correction** relative to whatever
offset was already stored (confirmed algebraically: the average equals
`B−O` or `O−B`, where `B` is the true hardware bias and `O` is the
pre-existing offset — never `B` alone). `CAL:` was structurally the
wrong command for what this value represents.

## The fix
Send the exact same correction values via `TAR:` (additive) instead of
`CAL:`. The firmware then adds the correction to whatever it currently
has: `pitchOffset_new = O + (B−O) = B`; `rollOffset_new = O + (O−B
negated) = B` — isolating the pure hardware bias exactly, with **zero
dependency on the app knowing `O`**. Verified numerically with two
different, deliberately untracked `O` values before implementing; both
converged to the identical, correct bias either way — confirming this
works regardless of an earlier Tare, a persisted offset from a prior
session, or whether this is the first calibration in a session.

This eliminates the entire `knownPitchOffsetRef`/`knownRollOffsetRef`
tracking mechanism v1.6 added — removed entirely, along with its usages
in Tare, Reset, and disconnect handling. **Net simpler than v1.6, not
just more correct.**

## Also updated
The calibration-complete message now clarifies that Calibration removes
sensor/mounting bias, distinct from Tare (which zeroes to a specific
surface) — per a testing-methodology note from the same review: "return
the level to its original position; Tare afterward to treat that surface
as zero."

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, FRONT/REAR bubble-mimic UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — still needs Calibration ported, using this same, simpler TAR:-based approach

## Known gap
No `icons/` folder included — same as every other package in this family.
