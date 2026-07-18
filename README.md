# Digital Level Tradesman 4 Point — Web App

**Revision 4** (based on v1.0) · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## Sharper evidence behind this revision
Closer comparison against DL Tradesman (standard) found that product
sends these **exact same two commands** — `CAL:0.00,0.00` and
`CAL:<value>` — elsewhere in its own code (a "Reset" action, and the end
of its 2-position Calibration flow), but **never as an automated pair**.
Its Calibration flow alone has a 3-second countdown before anything else
happens, followed by ~6 seconds of sample collection before its own
`CAL:` command is sent. Nothing in this product family has ever tested a
gap as short as Rev 3's 300ms between two `CAL:`-family commands.

## What changed in Rev 4
Raised the wall-clock delay between the clear write succeeding and the
app trusting subsequent packets from 300ms to **3000ms** — a floor
directly matched to the shortest known-good gap actually present in a
working flow in the sibling product (its 3-second countdown), not an
arbitrary increase. This is the only change in this revision.

**Verified before shipping:**
- The longer delay does not conflict with the existing
  `TARE_TIMEOUT_MS=3000` guard — that timer only starts *after* this new
  delay completes, so it cannot fire prematurely mid-wait.
- The double-tap guard still correctly blocks a second tap during the
  now-longer wait.

## Honesty about confidence level
This is a more strongly evidenced value than Rev 3's 300ms, but it's
still a floor/starting point, not a measured requirement.
- **If this resolves it** — the true minimum needed gap is still unknown
  (could be shorter than 3s).
- **If it does not resolve it** — that's meaningfully stronger evidence
  against the timing theory in general than Rev 3's result alone, since
  this gap is now backed by a known-good comparison point rather than an
  arbitrary guess.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
