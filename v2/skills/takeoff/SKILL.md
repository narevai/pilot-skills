---
name: takeoff
description: Fly the Zibo 737-800X takeoff roll, rotation and initial climb to 400 ft, then hand the airplane to CMD A. Use from brake release until the autopilot is engaged, or when you start on the runway or in the first minute of the climb. Time-critical: read this before the brake release, not during the roll.
---

# Takeoff

This phase lasts about one minute and is the one that crashes airplanes. You fly it by hand through slow tool calls, so the plan matters more than technique. Prepare everything in `before-takeoff` and read this skill before the brake release.

## Why it is hard

- **The airplane is faster than your loop.** With TO/GA it gains 6 to 8 kt per second and reaches VR 15 to 20 seconds after N1 passes 90 percent. A tool call takes 3 to 10 seconds, so the speed you read is old when you act on it.
- **`pitch-control` is the elevator position, not an angle and not a rate.** It is a fraction of full travel from -1 to 1 (positive raises the nose) and stays where you put it. Its effect grows with airspeed, so the same value does far more fast than slow: a modest input at 195 kt gave +10,000 ft/min and a stall. Any value in between is accepted, so you can make it as small as you need. The right size is not known in advance: find it from what the airplane does.
- **The failures seen so far are all the same.** Pilots made other calls during the roll, first rotated at 175 to 195 kt, pulled hard, stalled, pushed hard the other way and oscillated between a 10,000 ft/min climb and a dive until they crashed.

## Briefing (before the brake release)

Have these in your head and say them in your reasoning: runway heading, `v-speeds.vr`, `v-speeds.v2`, the cleared altitude, and the plan below. All checklist items for the takeoff section are marked before the roll (see `before-takeoff`); after the brake release you make **no** `mark_checklist`, MCP, light or reading-group calls until the gear is up.

## The roll

1. Check the gate in `before-takeoff` ("Ready to roll"). Release the parking brake (`parking-brake-toggle`; a released brake reads about 0.2, not 0).
2. Press `to-ga-left` once (`cockpit_engines_and_thrust`) with both levers at idle and the autothrottle armed. Both engines go to about 98 percent N1 in about 15 seconds. Do not move the levers by hand afterwards: it disarms the autothrottle.
3. Scan with **one call per cycle**: `read_group` on `flight-instruments` (heading, pitch, airspeed, height-agl, vertical-speed). Numeric readings carry `trend` (change per second over the last 5 s). Time to VR is `(VR - airspeed) / trend`. Read the N1 values once in the first cycle (`engines-and-thrust`) and expect them to match.
4. Keep the runway heading with small `rudder-control` inputs against the drift (read `heading` in the next cycle; return to 0 when straight). The autopilot does not steer the nosewheel and HDG SEL does not either.
5. Abort (both levers to idle with `throttle` 0, then stop; no rotation) if: the heading is more than about 5 degrees off the runway heading before rotation and not recovering, the engines are split or an N1 is not what you commanded, the nose stays on or below the horizon through rotation speed (the stabilizer is wrong; do not fight it with pitch control), or the readings fail. Do not add power to fly out of a heading change.

## Rotation

- Start the rotation a little before VR: your call lands a few seconds after you decide. Use the airspeed `trend` and begin when the time to VR is about as long as one call takes. Do not wait to read VR exactly.
- Make the first input small. Then read `pitch` and its `trend` in the very next cycle. Target **15 degrees**; ease the input back toward 0 as the pitch approaches it. Fifteen degrees is a target, not a pull through it.
- **Read the instruments every cycle, from the brake release until the autopilot is on, and base every input on the reading you have just made**, never on the previous input or on what you expect the airplane to do. The airplane keeps reacting to an input you wrote earlier.
- One change per read, and never a large one: its effect grows with airspeed. If you are late (fast), the airplane is already flying and a small input is enough.
- If pitch passes 20 degrees or the airspeed is falling, ease the input toward 0 gradually. Do not jump to a large value on the other side. If it still gets worse, use `stall-and-upset-recovery`.

## Liftoff to 400 ft

1. Positive climb (`vertical-speed` positive and `height-agl` rising): gear up (`gear` 0). Read the `*-gear-down` / `*-gear-transit` readings in a later cycle.
2. Keep pitch near 15 degrees and the heading on the runway heading with small inputs. Speed settles near V2 plus 15 to 20 kt.
3. At **400 ft `height-agl`**, with wings level and pitch stable:
   1. Roll mode: press `hdg-sel` alone and read `fma-roll` until it shows `HDG SEL` (TO/GA replaces it). For a route flight with the FMC route active and the track within the geometry, `lnav` instead; never `lnav` when the FMC has no route.
   2. Engage `cmd-a` once and read `cmd-a` (it is a toggle; do not press it again). Read `fma-pitch` and `fma-roll`.
   3. As soon as `cmd-a` is on, stop writing `pitch-control` and `roll-control` and turn `joystick-override` off. A pitch command sent while CMD A is engaging fights the autopilot.
   If CMD A will not engage, stay on the flight director with small inputs (never above the limits above) and keep the heading; do not hand-fly beyond 1,500 ft without reporting it with `report_issue`.

Engaging at 400 ft is the real-world limit and the right way out of the manual phase. The manual's takeoff checklist lists the autopilot "as needed" after flap retraction: engaging earlier is allowed, mark that item when you have done it.

## Flaps, speed and pitch mode

- Above 400 ft retract the flaps on the schedule (`flaps` 1, then 0; flaps 5 reads 37.5 percent on `flap-lever`, flaps 1 reads 12.5). Keep the MCP speed (`set-speed`) at about 160 kt until they are up; do not select 250 earlier.
- After the flaps and slats are up, select the pitch mode, one press, then read `fma-pitch`: `vnav` for a route flight (if `fma-pitch` does not become VNAV, because performance data or the cruise altitude is missing or the nav data is out of date, press `lvl-chg` instead; it annunciates as `MCP SPD`), `lvl-chg` for a straight-out climb.
- Never press `alt-hld` below the MCP altitude. Once `ALT HOLD` or `MCP SPD` shows, do not press `lvl-chg` again.
- If `fma-autothrottle` goes blank, re-arm it once (read the state first, it is a toggle) and read the annunciator. Do not press TO/GA to restore it.

Continue with `climb-and-cruise` (or finish here if the task ends at this point).

## Mark the checklist after the gear is up

Read the `takeoff` section again and mark every item you did and confirmed during the roll and climb (rotation, gear, roll mode, flaps, VNAV or LVL CHG, autopilot). Never mark one you did not do.
