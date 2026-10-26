---
name: before-takeoff
description: Prepare the Zibo 737-800X on the ground for takeoff and decide whether it is ready to roll: weight-dependent data, flaps, stabilizer trim, rudder and aileron trim, control zeroing, MCP, flight director, autothrottle, autobrake, transponder, Master Caution and the takeoff checklist. Use when the engines are running and the airplane is on the ground before the brake release.
---

# Before takeoff

Everything the roll needs is set and verified here, because during the roll you have no time for anything else (see `takeoff`). The FMC route and performance data come from `fmc-setup`. Read the `takeoff` checklist section (`read_checklist`) now and mark each item as soon as you have done and confirmed it. Do all the marking before the brake release.

## 1. Data

Read `takeoff-data` (`gross-weight`, `fuel-total`, `takeoff-flaps`, `flaps-match-fmc`, `trim-calculated`, `trim-matches-fmc`, `v1-set`, `vr-set`, `v2-set`, `fmc-cg`, `actual-cg`) and `v-speeds` (`v1`, `vr`, `v2`).

- V1, VR, V2 and the trim must be non-zero and calculated for this weight. If the CG shows `--.-%`, `trim-calculated` is 0 or a set V-speed is 0, the takeoff data has not been entered: do step 10 of `fmc-setup` first.
- Never reuse fuel, cruise altitude or V-speeds from a previous flight without checking them. Write down VR and V2.
- Get the charts: `list_documents` with kind `chart` and the airport ICAO, then `read_document`. Without ATC use an initial altitude consistent with the published departure restrictions and terrain.

## 2. Configuration

1. Flap lever to the takeoff flaps (`flaps`) and the stabilizer to `trim-calculated` (`set-stabilizer-trim`, once, value in units; the server trims by itself, never click trim in a loop). Verify `flight-controls.stabilizer-trim`. `flaps-match-fmc` and `trim-matches-fmc` must be true.
2. Do not use `pitch-trim` (the elevator trim ratio) to set the takeoff trim: a ratio of 0 reads 0 while the stabilizer stays wherever it was, and the airplane then rotates itself early and violently. The stabilizer units are what count.
3. Autobrake RTO (`autobrake` 0), speedbrake armed (`speedbrake` 1), transponder TA/RA (`transponder-mode` 5), lights as the checklist asks.
4. Flight directors on and autothrottle armed. These are toggles: read the state with `get_function_states`, press only if off, read again. Arming the autothrottle twice switches it off.
5. `master-caution` must read false: press `master-caution-captain` and read again. No six pack lamp may be lit without an explanation (see `cold-and-dark-start`).

## 3. Controls

The pitch, roll and rudder setpoints reach the airplane only while `joystick-override` is on.

1. Turn `joystick-override` on (toggle: read it first) and read it back.
2. Read `pitch-control`, `roll-control`, `rudder-control` and both `throttle` readings. All must be 0; set any other to 0 and read again. A fresh run can keep the previous attempt's values.
3. Read `rudder-trim` and `aileron-trim`: both must be 0. A rudder trim of -1 (full left) sent a takeoff off the runway with the pedals at 0 and the engines matched. No action sets these trims: if one is not 0, call `report_issue` and ask the user; do not take off.
4. Test the pedals: set `rudder-control` to 0.2, read it back and `rudder-input` (the real pedal position), then set it to 0 and read 0 again. Do not roll with the test deflection in.

## 4. MCP

- Heading: read `flight-instruments.heading` while stopped on the runway; that is the runway heading. `set-heading` to it and read `selected-heading`.
- Altitude: `set-altitude` to the cleared altitude, read `selected-altitude`.
- Speed: `set-speed` to the V2 you read, read `selected-speed`.
- Do not press `lnav`, `vnav`, `lvl-chg` or `alt-hld` on the ground: they do not arm, hold the field altitude, or command climb thrust before you are straight. Do not press `hdg-sel` now: TO/GA replaces it; you select it at 400 ft (see `takeoff`).

## Ready to roll

Roll only if all of these are true. Otherwise fix it or report it.

- V1, VR, V2, takeoff flaps and stabilizer trim set and matching the FMC
- Rudder and aileron trim 0, pitch, roll and rudder control 0, throttle at idle, `joystick-override` on
- Heading, altitude and speed set in the MCP; flight director on; autothrottle armed (`autothrottle-armed` true)
- Master Caution clear; the takeoff checklist items done so far are marked
- You know VR, V2, the runway heading and the cleared altitude

Then continue with `takeoff`.
