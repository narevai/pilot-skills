---
name: final-and-landing
description: Fly the final approach and landing of the Zibo 737-800X: LOC and G/S capture, the second autopilot, the hands-off rules that keep the glideslope, the autoland checks, flare, rollout, stopping and vacating the runway, and when to go around. Use from the APP press until the airplane is stopped clear of the runway.
---

# Final approach and landing

`descent-and-approach` brings you to the APP press. This skill takes over from there.

## Autoland needs both autopilots

A coupled ILS with CMD A only tracks LOC and G/S to minima but does not flare or roll out: you land it by hand and tell the user the landing needed manual input. Do not call it an autoland. A proper autoland needs CMD A and CMD B engaged and FLARE and ROLLOUT armed.

## Capture

1. Before APP: Nav 1 and Nav 2 on the same ILS frequency, identifier checked against the chart, both courses set (`nav-1-course`, `nav-2-course`), flight directors on, autothrottle armed, flaps to schedule and one `set-speed` to the approach speed. MCP altitude stays at the intercept altitude until the glideslope is captured.
2. `app` once. LOC captures (`fma-roll` `VOR/LOC`), then G/S from below (`fma-pitch` `G/S`) and the descent begins. Do not intercept the glideslope from above and do not press APP repeatedly; if the capture does not happen, go around.
3. After stable G/S capture and a steady descent, set the missed-approach altitude once (`set-altitude`). Not before: with G/S lost and the MCP above the airplane, the autopilot climbs to the MCP altitude.

## Second autopilot, once

A short press of `cmd-b` only changes over from CMD A to CMD B: CMD A goes off and you never get two channels. The second autopilot needs a **long press**: use `cmd-b-hold` (press and hold), with CMD A flying and APP armed, right after G/S capture and well above 1,500 ft (it is locked out below 800 ft radio altitude). Read `cmd-a` and `cmd-b`: both on. If CMD B was engaged first, use `cmd-a-hold` instead. **Never press either CMD button again**, short or long (they toggle). If one drops on final, go around; do not toggle to restore it. If both lights are not on after one `cmd-b-hold`, do not press again: continue the single-autopilot ILS and land by hand.

## Hard rules on final

| Do | Do not |
| --- | --- |
| Gear, landing flaps, speedbrake armed, autobrake, one `set-speed` | Press `cmd-a`, `cmd-b` or a hold variant again |
| One `cmd-b-hold`, one missed-approach `set-altitude` after G/S is established | Press `app`, `lnav`, `vnav`, `hdg-sel`, `lvl-chg` or `v-s` |
| Monitor airspeed, vertical speed (negative), height AGL, FMA, CMD lights, LOC and G/S dots | Fix a lost G/S with V/S inputs close in |

This is how a glideslope was lost on a flown approach: extra CMD presses dropped a channel and broke G/S, the MCP altitude was above the airplane, and it climbed. That is mode behaviour, not a mystery.

If the vertical speed turns positive on final, read `selected-altitude` and the FMA first. MCP above you and G/S gone means the autopilot is climbing to the MCP: fly the published missed approach with `to-ga-left`; do not force a late descent inside 3 to 4 nm when high.

Allowed on final: gear 2, flaps to schedule, `speedbrake` 1, engine start selectors to CONT (`engine-1-start`, `engine-2-start` value 2), one speed set, one `cmd-b-hold`, one missed-approach altitude, `to-ga-left`. Read the result of each before anything else.

## Autoland verification

At about 1,500 ft radio altitude read `height-agl` and `mcp-and-autopilot`: `fma-roll` `VOR/LOC`, `fma-pitch` `G/S`, `fma-pitch-armed` `FLARE`, `fma-roll-armed` `ROLLOUT`, both CMD lights on, `fma-autothrottle` engaged. There is no reading for the PFD LAND 3 annunciation; rely on these. **FLARE must be armed by 350 ft radio altitude**, otherwise treat it as an autoland failure and go around (or land by hand if you can do that with confidence).

## 1,000 to touchdown

Do not touch the MCP modes. Finish the configuration: gear down (`gear` 2), landing flaps (`flaps` 30 or 40), speed at VREF plus the correction (one `set-speed`), `speedbrake` 1, autobrake selected (`autobrake` 2, 3 or 4 = settings 1, 2, 3; read `autobrake-position`).

Then hands off: about 50 ft the AFDS flares (`fma-pitch` `FLARE`), about 27 ft the autothrottle retards, and `ROLLOUT` tracks the centreline after touchdown. Do not disconnect the autopilot, do not flare or pull the throttles yourself. Even with both CMD lights on, FLARE may not arm: if it is not armed by about 350 ft expect a firm touchdown or go around.

## After touchdown

You own the stopping.

1. Verify the speedbrakes deployed (`speedbrake-lever` near fully up).
2. `reverse-thrust` (`cockpit_engines_and_thrust`) holds maximum reverse for 5 seconds then releases; repeat while `ground-speed` is above about 60 kt and watch `reverser-left` and `reverser-right`. If the reversers do not spool (N1 stays near 60 percent and the speed rises), set `throttle` 0 and use the brakes. Never in the air.
3. The autobrake slows the airplane (`autobrake-position`). Use `brakes` or `brakes-max` only if it is not decelerating enough; manual braking can disarm it.
4. The autopilot disconnects on touchdown (both CMD lights off). Take the rudder immediately: turn `joystick-override` on, `rudder-control` with small values (check `heading` after the first input, correct, return to 0 to go straight). Without pedal input the airplane drifts off a narrow runway.
5. `ap-disconnect` if CMD is still on once the airplane is slow and straight. Never on approach (to give up an approach use `to-ga-left`).

## Vacate

Speed 10 to 20 kt before a turn (`throttle`, `brakes`). Follow the chart for the exit, keep the centreline until you turn, stop with the parking brake (`parking-brake-toggle`, read first) clear of the runway. Report where you are; the phase becomes `arrived` by itself.

## Go around when

A CMD drops, LOC or G/S is lost, or the FMA disagrees with the lights; FLARE is not armed by 350 ft and you cannot land by hand with confidence; the approach is unstable by 1,000 ft AGL; the required visual references are absent at minima. Use `go-around`. Do not experiment.
