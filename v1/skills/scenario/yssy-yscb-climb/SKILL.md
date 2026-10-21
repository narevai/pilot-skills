---
name: yssy-yscb-climb
description: Take off from Sydney YSSY runway 16R in the Zibo 737-800X and climb to cruise on the YSSY to YSCB route, with the FMC route preloaded. Use when the scenario ends at cruise and there is no descent or landing.
---

# Sydney to Canberra, takeoff to cruise

This is the first half of the Sydney to Canberra flight. The airplane is on runway 16R at YSSY with the engines running and the FMC loaded, exactly as in the `yssy-yscb` skill. Fly with `yssy-yscb` (sections "Before flight", "Departure and climb" and "Cruise and descent" up to the cruise), the `takeoff-straight-ahead` skill for the runway roll, and `fly-the-airplane` for how the tools work. Select both scenario skills when queueing.

## What is different

- **The run ends at cruise.** It stops by itself when the airplane is level above 3000 ft for about a minute (the phase `cruise`, shown by `get_run`). Do not start a descent, do not set up an approach and do not try to land; there is no landing in this scenario.
- **The checklist** covers only takeoff and climb/cruise (`read_checklist` with `category` `takeoff` and `climb_cruise`). Read each section when you reach it and mark the items you did and confirmed.
- The run has a shorter time limit than a full flight (about 30 minutes from the start of the pilot). Do not waste time before the takeoff roll: the checks in "Before flight" are quick reads, not a long procedure.
- Keep LNAV and VNAV armed and climb to the cruise altitude set on PERF INIT (FL220), with the MCP altitude set high enough to allow it.

If something prevents the climb (a failed mode, a wrong FMC entry), say what happened instead of improvising; the run will end at the time limit.
