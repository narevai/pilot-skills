---
name: climb-and-cruise
description: Climb the Zibo 737-800X after the autopilot is engaged and hold cruise with LNAV, VNAV or HDG SEL and LVL CHG, autothrottle and CMD A: gear and flap retraction, MCP altitude management, speed, transition altitude, route progress and fuel checks. Use when airborne above 400 ft and below or at the cruise altitude.
---

# Climb and cruise

Start here once `cmd-a` is on (see `takeoff`). If you are above 400 ft but hand-flying, engage `cmd-a` as soon as you are stable and read `stall-and-upset-recovery` if you are not. Read the `climb_cruise` checklist section when you reach it and mark items as you confirm them, in quiet moments.

## Choose the modes

- **Route flight** (the FMC holds an active route without discontinuity ahead, performance data entered): `lnav` and `vnav`. Confirm them armed (`fma-roll-armed`, `fma-pitch-armed`), then active.
- **No route, or the task gives headings and altitudes**: `hdg-sel` and `lvl-chg` (`fma-pitch` shows `MCP SPD`). Never press `lnav` or `vnav` when the FMC has no route.
- VNAV needs performance data, a cruise altitude and current nav data. If it will not engage or does not level at the MCP altitude (for example with `NAV DATA OUT OF DATE`), use `lvl-chg` deliberately and read `fma-pitch`.
- Do not repeat mode presses to "make sure": read the FMA after each one.

## Climb

- Gear up after a positive rate of climb, flaps on the schedule (`flaps` 1, then 0); keep the MCP speed near 160 kt until the flaps are up, then let the pitch mode set the speed; do not select 250 kt before they are retracted. Verify climb thrust (`fma-autothrottle`).
- Keep the MCP altitude at the cleared altitude and no higher than the restrictions allow. If the airplane has levelled at the MCP altitude, a higher selection may need VNAV or LVL CHG again; confirm the resulting mode.
- Do not use `altitude-intervention` or `speed-intervention` repeatedly: they delete restrictions.
- At the transition altitude set standard pressure: `cockpit_altimeter` `std`, then read `altimeter.std-selected`.
- A pitch mode, an altitude capture and a speed change are slow. After each action look at `vertical-speed` and its `trend` before the next one.

## Cruise

- Maintain the modes. Check route progress (`latitude` / `longitude` against the chart or the FMC PROG page), the fuel remaining, and the weather at the destination (`read_group` on `weather`).
- The phase becomes `cruise` when the airplane is level at altitude. A task that ends at cruise stops by itself.
- Plan the descent before the top of descent and continue with `descent-and-approach`.

## If the autopilot drops or misbehaves

Read `cmd-a`, `cmd-b`, `fma-roll` and `fma-pitch`. Do not press the CMD buttons repeatedly. If it is off and the airplane is stable, re-engage `cmd-a` once with `joystick-override` off and stop touching the stick. If it is off and the airplane is not stable, `stall-and-upset-recovery`.
