---
name: stall-and-upset-recovery
description: Recover the Zibo 737-800X when it is not doing what you commanded: pitch above 25 degrees, airspeed falling toward stall, a dive, bank above 45 degrees, or a pitch oscillation caused by your own control inputs. Use immediately, before anything else, when the readings show an upset.
---

# Stall and upset recovery

Typical causes here: a late or large `pitch-control` input on takeoff, a hand-flown climb that bled the speed, a mode that changed under you, or inputs that chase a reading that is already old.

## Recognise it

Any of: `pitch` above 25 degrees or below -10; `airspeed` falling with a high pitch or the trend strongly negative; `vertical-speed` far from what you commanded (+5,000 ft/min at low speed is a zoom that ends in a stall); roll (bank) beyond 45 degrees; or pitch changing sign between two reads (an oscillation).

## Rules

- **The stick position is a held value.** Read `flight-controls.pitch-control` first. A large value left in from the last cycle is usually the cause and the first fix is to take it out.
- **Reduce the angle of attack before anything else.** Lower the nose by moving `pitch-control` toward 0 and slightly negative (start with -0.10 to -0.15). Read `pitch` in the next cycle. Do not use -0.3 or more: -0.25 took a stalled climb into a -16 degree dive into the ground.
- **One change per read.** When pitch is moving the right way and the airspeed trend turns positive, go back to 0. The effect of an input grows with airspeed, so inputs that were right at 100 kt are too large at 180.
- **Wings level.** If the bank is above 30 degrees, a small `roll-control` against it (start at 0.1 to 0.2 opposite the bank; check the sign on the first input), read `flight-instruments.roll`, return to 0.
- **Thrust.** In a stall at low altitude use `to-ga-left` for takeoff thrust (one press) if the autothrottle is armed; do not move the levers by hand. Do not reduce thrust to "slow down" in a dive unless the speed is high.
- **Do not pull out of a dive hard.** Level the wings first, then raise the nose with small inputs toward 5 to 10 degrees pitch. A pull-out that stalls again is how the oscillation continues.

## After recovery

When `pitch` is within about 15 degrees, wings are level and the airspeed is above V2 plus 20 kt and rising:

1. Read the FMA and set the modes you want, one at a time: `hdg-sel`, `lvl-chg`, the MCP altitude (`set-altitude`) above the airplane's current altitude. `alt-hld` only if you want to hold the current altitude.
2. Engage `cmd-a` once, read it, then stop writing `pitch-control` and `roll-control` and turn `joystick-override` off.
3. Tell the user what happened with `report_issue` if the cause was a tool, a reading or a procedure.

If `height-agl` is too low and the airplane is still sinking, there is nothing left to try but the rules above, applied once per read. Do not make large inputs to catch up.
