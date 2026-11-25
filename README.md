# Digital Level Tradesman 4 Point — Web App

**Version 1.11 — Test Candidate** · 2026-11-24 · MPU-6050 · Arduino Nano ESP32 · BLE Nordic UART

Built on top of the locked Proto 1.0. Not yet re-locked — pending your
confirmation before it replaces Proto 1.0 as the shipping version.

## v1.11 changes
Bubble Demo pointer capture and mouse/touchpad drag handling; touchscreen support retained. Chromebook and hardware regression tests pending. Not released.

## What's new in v1.10
Adds the **Slope / Degree Calculator**, ported from Digital Level
Tradesman (standard), where it already exists and works. Missing from
DLT4P was an oversight in how the 4-point UI was originally built, not
a deliberate omission — flagged during a product evaluation for
Facilities Logistics, who received a prototype without it.

## Confirmed UI-only before porting, not assumed
The component carries its own local state (`calcMode`/`calcInput`) and
has zero dependency on sensor data, BLE connection, or this product's
pitch/roll specifics — confirmed by reading the standard product's
actual component before writing anything.

## Style adapted, logic copied straight
The standard product's version uses Tailwind `className` strings and a
teal accent; this file uses inline style objects throughout and DLT4P's
own established amber accent (`#fbbf24`) everywhere else in the app.
Converted to match — teal would have been a visually inconsistent
one-off in an otherwise fully amber-themed app.

## Verified with exact math, not just a visual check
Rendered headless and tested both calculation modes against known exact
values before considering this done:
- **45°** → **120.000″** rise, **100.000%** grade (tan(45°) = 1 exactly)
- **12 in/ft** → **45.0000°** (atan(12/12) = 45° exactly)

No page errors in either direction.

## Placement
Directly after Calibration, before the footer — matching where it sits
in the standard product's own layout and guide ordering. Independent of
connection state, same as there.

## Family naming (current)
- **Digital Level RV** — MPU-6050, 4-point bull's-eye UI, RV market
- **Digital Level Tradesman** — MPU-6050, FRONT/REAR bubble-mimic UI, general trades
- **Digital Level Tradesman 4 Point** (this product) — MPU-6050, 4-point bull's-eye UI, general trades / machine leveling
- **Digital Level Pro** — ADXL355, standard bubble UI, precision/millwright
- **Digital Level Pro 4 Point** — ADXL355, 4-point bull's-eye UI, precision/millwright — also missing the Slope/Degree Calculator, same fix applies

## Known gap
No `icons/` folder included — same as every other package in this family.
