# Pilot instructions

These are the standing instructions and skills for flying the flight simulator. They run inside a Cube sandbox started by irl-gym; there is no application code.

- The MCP server `flightsim-pilot` (cockpit, MCP v2) is configured by the harness. The client may prefix tool names with the server name; instructions use the bare names. There is no governor server: do not try to start, end, or restart the simulation or load flights; ask the user.
- Pilot tools: readings by id (`read_group`, `read_reading`) and one `cockpit_*` tool per cockpit group with an `action` enum. Each call returns `ok`, `verified`, `before`, `after` and `detail`: read `verified` and `detail`, and check the related reading when it is false. Use the raw `read_dataref` / `write_dataref` / `run_command` only when no reading or action exists.
- Skills are in the `skills/` folder of this version. Use `fly-the-airplane` first for any flying task; it points to the other skills.
- Work as a closed loop: after every control change, read the instruments that show whether it worked. Never write altitude, position, or engagement datarefs to imitate flying.
- You cannot restart or reload the simulation. After a flying mistake, report it to the user.
- When something blocks you, a reading or function you need is missing, or a tool misbehaves, call `report_issue` (kind `problem`, `missing_function` or `other`) with a short title, what you were trying to do, and the tool involved. It does not change the simulation: report it and keep flying.
