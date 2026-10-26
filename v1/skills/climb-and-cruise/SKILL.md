---
name: climb-and-cruise
description: Climb the Zibo 737-800X after takeoff and hold cruise with LNAV, VNAV, autothrottle and CMD A: 400 ft handover, gear and flap retraction, MCP altitude management, route progress and fuel checks. Use when airborne below or at the cruise altitude.
---

# Climb and cruise

Use `fly-the-airplane` for how the tools work. Read the `climb_cruise` checklist section (`read_checklist` with `category`) when you reach it, and mark items as you confirm them.

## Climb

- At or above **400 feet height AGL**, once stable, trimmed and following flight director guidance, arm `lnav` when the geometry permits and `vnav`, confirm them armed (`fma-roll-armed`, `fma-pitch-armed`), then engage `cmd-a`. Verify CMD engagement and the lateral and vertical modes. If there is no route in the FMC, use `hdg-sel` and `lvl-chg` instead and fly the headings and altitudes the task gives.
- `pitch-control` is the elevator position, held and sensitive (see `fly-the-airplane` and `takeoff-straight-ahead`): steps of 0.05 to 0.15, read `flight-instruments.pitch` at once, and stop writing it once `cmd-a` is on.
- Retract the gear after a positive rate of climb (`gear` value 0) and the flaps on the scheduled speeds. Verify climb thrust.
- Manage the MCP altitude through the climb so that it permits the cleared climb and keeps the restrictions. If the airplane levels at the MCP altitude, selecting a higher one may need the appropriate VNAV resumption. Confirm the resulting mode before continuing.
- Set standard pressure at the transition altitude: `cockpit_altimeter` `std`, then read `altimeter.std-selected`.

## Cruise

- Maintain LNAV and VNAV. Check the route progress with `latitude` / `longitude` against the chart or the FMC PROG page, the fuel remaining, and the weather at the destination (`read_group` on `weather`).
- The flight phase becomes `cruise` when the airplane is level at altitude. A task that ends at cruise stops by itself: do not continue past what it asks for.
- Plan the descent before the top of descent: continue with `descent-and-approach`.
