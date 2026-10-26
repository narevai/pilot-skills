---
name: fly-the-airplane
description: How the flightsim-pilot (v2) MCP tools work: readings, trends, cockpit actions, toggles, verification, the checklist that scores the flight, and what to do when something blocks you. Use at the start of every task and whenever a tool behaves unexpectedly.
---

# Fly the airplane: the tools

Work a closed loop: act, read the result, read the instrument that proves it. The skills for each phase say what to do; this one says how the tools behave. Tool names here are bare; the client may prefix them with the server name (`flightsim-pilot`). Read each tool's schema before you call it.

## Time is part of the loop

Every call takes seconds (a cockpit action about 0.4 s plus your own thinking). The airplane does not wait. Use few calls per cycle: `read_group` returns a whole group in one call. In a fast phase (takeoff roll, short final) read one group, decide, make at most one control call. Never batch a control change together with a mode button.

## Readings

- `list_groups`, `list_readings`, `read_group`, `read_reading`. Ids look like `flight-instruments.airspeed` or `mcp-and-autopilot.cmd-a`. Groups: `v-speeds`, `takeoff-data`, `fmc-cdu`, `flight-instruments`, `altimeter`, `weather`, `radios-and-ils`, `runway-and-approach`, `engines-and-thrust`, `brakes-flaps-and-gear`, `flight-controls`, `mcp-and-autopilot`, `lights-and-transponder`, `electrical`, `apu-and-fuel`, `hydraulics-and-air`, `anti-ice-and-wipers`, `engine-start`, `lights-and-signs`, `efis-captain`, `efis-first-officer`, `irs`, `display-panels`, `radio-panel-and-audio`, `tests-and-service`, `fire-and-emergency`.
- Numeric readings carry `trend` (change per second over `window`, 5 s), `ago` (the value `window` seconds ago) and `window`. They are null until enough history has been collected, so do not rely on a trend in the first seconds of a run. Use it to extrapolate: time to reach a speed or altitude is `(target - value) / trend`.
- On/off readings are booleans; mode readings (the FMA) are text.
- `get_function_states` returns on/off for every action that has an indicator (lights, switches, flap detents, MCP modes). It and some `read_group` calls intermittently answer `Simulator is unavailable`: retry the same call.
- Useful readings that are easy to miss: `flight-instruments.on-ground`, `height-agl`, `elevation-msl`, `brakes-flaps-and-gear.gear-handle-down`, `takeoff-data.takeoff-flaps`, `runway-and-approach` (reference airport and runway, course, length), `flight-controls.pitch-input` / `rudder-input` (the real control positions, as opposed to the setpoints you command).

## Cockpit actions

One tool per group (`cockpit_<group>`, dashes become underscores, for example `cockpit_engine_start`, `cockpit_electrical`, `cockpit_efis_captain`). Each takes `action` (an enum listed in the tool description with its kind, unit and range) and, depending on the kind, `value` or `text`.

- `press`: no value. `toggle`: flips the switch.
- `setpoint` and knobs: `value` in the listed unit; one call is enough, the server turns the knob until it matches. Stick and pedal setpoints (`pitch-control`, `roll-control`, `rudder-control`, `pitch-trim`) accept any value in -1 to 1.
- `select`: `value` is a position listed in the tool description (for example `flaps` 5, `gear` 2 = down, `transponder-mode` 5 = TA/RA, `autobrake` 3 = setting 2).
- `text` (`cdu-type` in `cockpit_fmc_cdu`): the characters to type, in one call.
- The result has `ok`, `verified`, `before`, `after` and `detail`. `verified: true` means the server saw the change. `verified: false` with `no change` or `no state feedback` means the command was sent but the effect was not seen: read the related reading before you assume it worked.
- **Toggles flip.** `flight-director-toggle`, `a-t-arm-toggle`, `parking-brake-toggle`, `cmd-a`, `cmd-b`, `joystick-override`, lights: read the state first, press once, read again. Pressing twice undoes it.
- Spring-loaded switches return to centre (fuel flow `used`, test switches, air valve manual `open`): a press is momentary, check the effect, not the switch.
- Slow switches (APU, generators) can take a couple of seconds to report.
- Pitch, roll and rudder setpoints reach the airplane only while `joystick-override` is on, and they are held until you change them. `pitch-control` is an elevator position, not an angle; its limits for takeoff are in `takeoff`.

Raw access (`read_dataref`, `write_dataref`, `run_command`, `search_datarefs`) is a last resort and may be switched off. Almost every value has a reading: look with `list_groups` and `list_readings` first. Never write altitude, position or engagement datarefs to imitate flying.

You do not control the simulation: there is no pause, resume, restart or reload tool. If the readings stay frozen (airspeed, altitude and `flight-instruments` never change between reads), say so and stop.

## The run

`get_status` (connection and aircraft) and `get_run` (the run, its phase, where it ends). The flight phase is worked out from the simulator and `set_run_phase` is not something you need. A run can end on its own (landing, crash, the scenario's end, the time limit): do not fly past what the task asks for.

## Documents and clearance

`list_documents` (category `aircraft`, `flight`; kind `manual`, `plan`, `chart`, `sop`) and `read_document`, `list_document_chapters`, `read_document_chapter`. Clearance (ATC) is not available from any tool: it comes from the user. A chart can be a PDF some models cannot read; use the numbers in the route skill and say so.

## Checklist (the flight is scored on it)

- `list_checklist_categories`, `activate_checklist`, `read_checklist` (`category`). The first scoped read returns `user_id`, `run_id` and `execution_id`; reuse them. If `user_id` is null there is no active run: you can read but not record.
- When you have done an item and seen the confirmation (a reading, `verified: true`, the FMA), mark it with `mark_checklist` (`section`, `item`). An item you did but did not mark counts as missed. Marks record your acknowledgement, not the simulator state.
- Mark during quiet moments: before the brake release, after the gear is up, in cruise. Never mark during the takeoff roll or short final.
- If you did something before you read its section, read the section and mark it. Before you move on, review the section you are finishing. Never mark an item you did not do, and never a whole section in one go.

## When something blocks you

A reading or function you need is missing, a tool misbehaves, a setting cannot be changed: call `report_issue` (kind `problem`, `missing_function` or `other`) with a short title, what you were trying to do and the tool involved. It does not change the simulation, so report it and keep flying. If the airplane crashes or the run is ending and you still have time, report what went wrong the same way.

## Skills

`assess-flight-state` decides what to do next. Then, in order of the flight: `cold-and-dark-start`, `fmc-setup`, `before-takeoff`, `takeoff`, `climb-and-cruise`, `descent-and-approach`, `final-and-landing`, `go-around`; `stall-and-upset-recovery` whenever the airplane is not doing what you commanded. Route skills (for example `yssy-yscb`) hold the chart numbers.
