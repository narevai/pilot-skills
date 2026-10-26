---
name: go-around
description: Go around from an ILS or other approach in the Zibo 737-800X. Use when approach guidance fails, a CMD drops on final, the approach is unstable by 1000 ft AGL, the required visual references are missing at the minima, or autoland does not arm or flare.
---

# Go-around

Decide early. Never keep descending because a command was accepted: confirmed guidance, or you take control.

1. `to-ga-left` in `cockpit_engines_and_thrust`, once. Read the result: thrust (N1), `fma-pitch`, `fma-roll`, `fma-autothrottle`.
2. If the autopilot disconnects, you fly: pitch up to about 15 degrees with small `pitch-control` inputs (the limits and the reason are in `takeoff`: the effect depends on airspeed and your calls are slow). If you get a pitch that grows or a speed that falls, `stall-and-upset-recovery`.
3. Flaps to 15 and verify a positive climb before gear up (`flaps`, then `gear` 0).
4. Follow the published missed approach: track, altitude, configuration. The chart is a document (`list_documents` kind `chart`); a route skill may list the numbers. Set the missed-approach altitude on the MCP.
5. After the flaps are retracted select `lvl-chg` and read `fma-pitch`.
6. The phase returns to `climb`. Read the situation again with `assess-flight-state`, then fly another approach with `descent-and-approach` or tell the user you cannot continue safely.

Issue each mode action once, observe its effect, decide the next action from the resulting state. Do not press CMD buttons repeatedly.
