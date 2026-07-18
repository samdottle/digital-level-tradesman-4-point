# Digital Level Tradesman 4 Point — Web App

**Version 1.4** · 2026-07-18 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

## What changed in v1.4
Fixed: tapping Tare while tilted significantly off level made the bubble
jump to the edge of the display and appear stuck there for a couple
seconds before slowly returning to center, instead of settling
immediately.

**Root cause:** Tare reset the displayed React state (`pitch`/`roll`) to
0, but never reset the EMA smoothing accumulator (`epRef.current` /
`erRef.current`) feeding it. That accumulator kept holding its stale
pre-tare value — e.g. tare from a 20° tilt: the next real packet arrives
near 0°, but even the fast adaptive alpha (0.30, added in v1.2) only pulls
the accumulator to `0.30×0 + 0.70×20 = 14°` on the first step — still
outside the ±5° display range, so the bubble clamped to the edge and
stayed there while it unwound over several more packets.

**Fix:** reset `epRef.current` and `erRef.current` to 0 in the Tare
handler itself, alongside the existing `setPitch(0)`/`setRoll(0)`.

## Important: this bug is family-wide, not specific to this product
This is **not new in v1.4**, and not even specific to Tradesman 4 Point —
it's been present since v1.0 (inherited from Pro 4 Point). The same
pattern (Tare resets displayed state but not the EMA accumulator behind
it) is confirmed present in:
- **Digital Level Pro**
- **Digital Level Tradesman**
- **Digital Level Pro 4 Point**

None of those were touched in this pass. Flagging clearly so this doesn't
get lost — recommend fixing all three the same way before shipping.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, standard bubble UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright

## Known gap
No `icons/` folder included — same as every other package in this family.
