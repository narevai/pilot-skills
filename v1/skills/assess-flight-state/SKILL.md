---
name: assess-flight-state
description: Work out what state the airplane is in and which skill to use next. Use at the start of every task, after any interruption, and whenever you are unsure whether the airplane is cold and dark, ready on the runway, climbing, cruising, descending or on final.
---

# Assess the flight state

The task tells you where you start and what to achieve. It does not tell you what the airplane looks like; read it. Change nothing until you have read the state.

## Read

1. `get_status`, then `get_run` (the flight phase and, if the run has one, where it ends). If `get_run` returns no run, tell the user; you cannot start one.
2. `read_flight_deck_indications` (flight instruments with `on-ground`, `height-agl`, `airspeed`, `altitude`, `vertical-speed`, `heading`, position; engines; flaps, gear and speedbrake; the FMA, CMD lights and selected altitude, speed and heading; nav 1 and 2).
3. `read_group` on `electrical`, `fmc-cdu` (is there a route?), `takeoff-data` (fuel, weight, V-speeds) and `weather`, which it does not cover.

The flight phase is worked out from the simulator, so you do not set it. Phases in order: `preflight` (stationary on the ground), `taxi`, `takeoff` (roll above about 40 kt), `climb`, `cruise`, `descent`, `approach` (within about 20 nm of the destination, descending), `landing` (gear down, below about 500 ft), `arrived` (stopped at the destination). A go-around returns the phase to `climb`. A phase changes only after its conditions have held for a few seconds, so it can lag the airplane.

## Which skill next

| What you read | State | Skill |
| --- | --- | --- |
| Nothing powered, engines off, parking brake set | Cold and dark | `cold-and-dark-start` |
| Powered, engines off or running, the FMC has no route or no performance data | Not set up | `fmc-setup`, then `before-takeoff` |
| On the ground, engines running, FMC set, not rolling | Ready to go | `before-takeoff`, then `takeoff-straight-ahead` |
| Rolling on the runway | Takeoff | `takeoff-straight-ahead` |
| Airborne, gear down or up, below the cleared altitude | Climb | `climb-and-cruise` |
| Airborne, level at altitude, far from the destination | Cruise | `climb-and-cruise`, then `descent-and-approach` |
| Airborne, descending or near the destination, ILS not captured | Descent or approach | `descent-and-approach` |
| LOC and G/S captured (`fma-roll` VOR/LOC, `fma-pitch` G/S) | Final | `ils-final-hands-off`, `landing` |
| Approach unstable or guidance lost | Go-around | `go-around` |
| On the ground after touchdown | Rollout and vacate | `landing` |

Use `fly-the-airplane` throughout for how the tools work and `routes/` skills when one of them describes the route you are flying.

## Take over, do not redo

When you start in the air or with the engines running, the airplane has been prepared by the simulator and not by you. Do not repeat start-up steps, and do not press toggles whose state you have not read. Anything the task needs that was not prepared (altimeter, radios, lights, minimums, autobrake, the FMC route) is yours: check each one before relying on it.

Stop where the task says to stop. Some tasks end early, for example when the airplane reaches cruise: the run stops by itself, so do not fly past what the task asks for.
