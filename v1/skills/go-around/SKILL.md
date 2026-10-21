---
name: go-around
description: Go around from an ILS or other approach in the Zibo 737-800X. Use when approach guidance fails, the approach is unstable by 1000 ft AGL, the required visual references are missing at the minima, or autoland does not arm or flare.
---

# Go-around

Decide early. If guidance fails, the approach becomes unstable, or the visual references are absent at the applicable minima, go around. Never continue descending simply because a command was accepted: establish confirmed guidance or take control.

1. `to-ga-left` in `cockpit_engines_and_thrust`. Verify thrust and the FMA (`fma-pitch`, `fma-roll`, `fma-autothrottle`).
2. Control the airplane immediately if the autopilot disconnects.
3. Follow the published missed approach: lateral path, altitude and configuration schedule. The chart is a `flightsim-pilot` document (`list_documents` with kind `chart`); do not invent the altitude. A route skill may list it.
4. Retract the gear after a positive rate of climb and the flaps on the scheduled speeds. Set the missed approach altitude on the MCP.
5. The flight phase returns to `climb`. Read the situation again with `assess-flight-state`, then fly another approach with `descent-and-approach` or tell the user you cannot continue safely.

Issue each mode action once, observe its effect, and decide the next action from the resulting airplane state.
