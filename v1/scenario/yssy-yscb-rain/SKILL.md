---
name: yssy-yscb-rain
description: Fly the Sydney YSSY to Canberra YSCB route in rain and low cloud with the Zibo 737-800X, FMC route preloaded as in the base scenario. Use when the weather group shows rain, reduced visibility, a low cloud base or a wet runway.
---

# Sydney to Canberra, in rain

The airplane, the FMC and the route are the same as in `yssy-yscb`; fly the flight with that skill and select both scenario skills when queueing. Only the weather differs: moderate to heavy rain, reduced visibility, a low overcast and a wet runway, with wind that changes with height.

Read the weather before each decision, not only at the start: `read_group` on `weather` (temperature, dew point, wind speed and direction, visibility, QNH, cloud base). The numbers are in the readings; do not assume them. There is no ATC and no ATIS.

## What to do differently

- **Before takeoff.** The runway is wet. Check the runway lengths and the takeoff performance in the airport documents (`list_documents`, `read_document`) and keep the flap and V-speed setup consistent with the FMC. Set the wipers (`cockpit_anti_ice_and_wipers`). Engine anti-ice is needed when the temperature is near freezing or low and there is visible moisture: read the temperature and decide with the manual (`list_documents`).
- **Wind.** Work out the headwind, tailwind and crosswind components for the departure runway (16R) and for the landing runway (35) from the wind readings in `weather`. If a component is outside the limits in the aircraft manual, do not continue silently: tell the user, who decides about another runway (the FMC route is built for runway 35).
- **Climb and descent.** Expect cloud from the base up to its top: keep the speeds and the flight director modes you would use in cloud, watch for turbulence and keep the seat belt sign on when it is rough.
- **Approach.** The visibility and the cloud base decide whether the approach is flown to the minima: check them against the ILS minima in `yssy-yscb` before descending below the intercept altitude. Do not descend below minima without the required visual references; go around instead (`to-ga-left`).
- **Landing.** The runway is wet: use a higher autobrake setting, reverse thrust and brakes as in `landing`, and expect a longer rollout. Steer with the rudder.

If a reading you need (visibility, cloud base, wind) is missing or looks wrong, say so with `report_issue` and keep flying.
