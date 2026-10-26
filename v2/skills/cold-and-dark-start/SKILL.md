---
name: cold-and-dark-start
description: Start the Zibo 737-800X from cold and dark: battery, APU, power, IRS alignment, air, engine start and clearing Master Caution. Use when the cockpit is dark, nothing is powered and the engines are off.
---

# Cold and dark start

Use `fly-the-airplane` for how the tools work. The airplane is parked with fuel loaded, the cockpit is dark and there is no ground power. This skill ends with both engines running and the airplane powered and configured; set up the route and performance data with `fmc-setup` and then continue with `before-takeoff`. IRS alignment takes minutes: start it early and work on the other steps while it runs.

## Rules for this start

- You cannot load fuel, ground power, or a route from outside. If a needed thing is missing (no fuel, a switch that never responds), say so and call `report_issue`.
- Do one thing at a time. After each action read the result (`verified`, `before`, `after`, `detail`) and the reading that proves it before moving on.
- The checklist starts at the preliminary preflight. Read the section (`read_checklist`), do the item, confirm it with a reading, then `mark_checklist`. Mark it as soon as it is confirmed, not later. Before you move on, review the section you are finishing and mark what you did. Never mark an item you did not do.

## 1. Look before touching

`get_status`, then `read_group` for `electrical`, `apu-and-fuel`, `engines-and-thrust`, `brakes-flaps-and-gear`, `takeoff-data`, and `weather`. Confirm: parking brake set, fuel on board (`takeoff-data.fuel-total` above zero), engines off, and nothing powered. Do not continue if there is no fuel.

## 2. Power

1. `cockpit_electrical`: `battery` 1, `standby-power` 1 (auto). Read `electrical` for the battery bus and DC bus readings.
2. `cockpit_apu_and_fuel`: left forward and left aft fuel pumps on (`left-forward-fuel-pump`, `left-aft-fuel-pump`), then `apu` 1 and `apu-start`.
3. Wait for the APU to run up (about a minute). Read `apu-and-fuel` and the AC and DC bus readings; do not use the APU generators before the APU runs.
4. `cockpit_electrical`: `apu-generator-1` 1 and `apu-generator-2` 1. Confirm the AC transfer bus readings.

## 3. Systems before engine start

1. `cockpit_irs`: set both IRS mode selectors to align and then nav (read the IRS readings; alignment takes minutes, so start it early and keep working).
2. `cockpit_hydraulics_and_air`: APU bleed on, isolation valve auto, both packs auto, yaw damper on, hydraulic pumps on. Read the duct pressure and pack readings.
3. `cockpit_lights_and_signs` and `cockpit_lights_and_transponder`: set the lights and signs the checklist asks for (seat belt sign, emergency exit lights armed, navigation and anti-collision lights as required).
4. `cockpit_efis_captain` and `cockpit_efis_first_officer`: baro unit, minimums, map range as the checklist and the weather require. Set the altimeter with `cockpit_altimeter`.

## 4. Engine start

Measured on this airplane; follow this order. Read the engine readings after each step (`read_group` on `engines-and-thrust`).

1. APU running and its generators on, `bleed-air-apu` on (`cockpit_hydraulics_and_air`), isolation valve auto, parking brake set, fuel levers in cutoff.
2. **Turn both packs off** (`left-pack` 0, `right-pack` 0) before the start. With the packs on, the duct pressure stayed near 20 psi and N2 stalled at about 17 percent; with them off it rose above 25 psi and the start went through.
3. `engine-1-start` to GRD (`cockpit_engine_start`, value 0). Watch `n2-left` rise (about 25 percent takes 10 to 40 seconds, depending on the duct pressure). At 25 percent N2 or more move `fuel-lever-1` to idle (value 1). EGT peaks near 900 °C and then falls; N2 settles near 60 percent and N1 near 19 percent. Then return `engine-1-start` to off (value 1).
4. Repeat for engine 2 (`engine-2-start`, `fuel-lever-2`).
5. After both engines run: `generator-1` 1 and `generator-2` 1, then `apu-generator-1` 0 and `apu-generator-2` 0, `bleed-air-apu` 0, packs back to auto (value 1). Read the bus readings.

Do not hurry: if a reading does not move, wait a few seconds and read again before repeating an action. If N2 stops rising below 25 percent, read the duct pressure first.

## 5. Master Caution and the six pack

A lit Master Caution on a cold cockpit is normal: it lights while systems power up and are not yet configured (fuel pumps, packs, hydraulics, APU). Clear it in this order.

1. After the power, air and engine steps read the six pack with `read_group` on `fire-and-emergency` (`six-pack-fuel`, `six-pack-flight-controls`, `six-pack-electrical`, `six-pack-air-conditioning`, `six-pack-ice` and the others, plus `master-caution`).
2. Fix the cause of each lit lamp (for example pumps off for fuel, packs off for air conditioning). A six pack lamp goes out only when its condition is gone.
3. Press `master-caution-captain` once (`cockpit_fire_and_emergency`) and read `master-caution` again. If it comes back, a condition is still present: read the six pack again.
4. The six pack buttons (`six-pack-captain`, `six-pack-first-officer`) only reset the alarm state; they do not hide a condition that persists.
5. Before taxi no six pack lamp may be lit without an explanation. If one stays lit and you cannot explain it, call `report_issue` with the lamp and what you tried.


## 6. Done

Both engines are running, the generators supply the buses, the packs are back on auto, Master Caution is clear and no six pack lamp is lit without an explanation. Continue with `fmc-setup` if the FMC has no route, then `before-takeoff`.
