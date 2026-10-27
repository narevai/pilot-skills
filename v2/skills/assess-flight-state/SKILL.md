---
name: assess-flight-state
description: Work out what state the airplane is in and which skill to use next. Use at the start of every task, after any interruption, and whenever you are unsure whether the airplane is cold and dark, ready on the runway, rolling, climbing, cruising, descending or on final.
---

# Assess the flight state

The task tells you where you start and what to achieve, not what the airplane looks like. Read it. Change nothing until you have.

## Read

1. `get_status`, then `get_run` (phase and, if the run has one, where it ends). If there is no run, tell the user; you cannot start one.
2. `read_flight_deck_indications` (flight instruments with `on-ground`, `height-agl`, `airspeed`, `altitude`, `vertical-speed`, `pitch`, `heading`; engines; flaps, gear and speedbrake; the FMA, CMD and selected values; nav 1 and 2).
3. `read_group` on `electrical`, `fmc-cdu` (is there a route?), `takeoff-data` (fuel, weight, V-speeds) and `weather`, which it does not cover.

Phases, in order: `preflight` (stationary on the ground), `taxi`, `takeoff` (rolling above about 40 kt), `climb`, `cruise`, `descent`, `approach` (within about 20 nm of the destination, descending), `landing` (gear down, below about 500 ft), `arrived`. A go-around returns the phase to `climb`. The simulator sets the phase after its conditions have held for a few seconds, so it can lag.

## Which skill next

| What you read | Skill |
| --- | --- |
| Nothing powered, engines off, parking brake set | `cold-and-dark-start` |
| Engines on or off, FMC without route or performance data | `fmc-setup`, then `before-takeoff` |
| On the ground, engines running, FMC set, not rolling | `before-takeoff`, then `takeoff` |
| On the runway rolling, or airborne below about 400 ft | `takeoff` (read the whole skill before acting) |
| Airborne above 400 ft, below the cleared altitude | `climb-and-cruise` |
| Level at altitude, far from the destination | `climb-and-cruise`, later `descent-and-approach` |
| Descending or near the destination, ILS not captured | `descent-and-approach` |
| LOC and G/S captured (`fma-roll` VOR/LOC, `fma-pitch` G/S) | `final-and-landing` |
| Unstable approach or guidance lost | `go-around` |
| On the ground after touchdown | `final-and-landing` (rollout and vacate) |
| Pitch above 25 degrees, airspeed falling, vertical speed far from what you commanded, roll beyond 45 degrees, or the airplane is not doing what you set | `stall-and-upset-recovery` first |

## Take over, do not redo

When you start in the air or with the engines running, the simulator prepared the airplane, not you. Do not repeat start-up steps and do not press toggles whose state you have not read. Anything the task needs that was not prepared (altimeter, radios, lights, minimums, autobrake, the FMC route, the checklist) is yours: check each before relying on it. If you start in the air with the autopilot flying, look first and leave what is right.

Stop where the task says to stop. Some tasks end early, for example at cruise: the run ends by itself.
