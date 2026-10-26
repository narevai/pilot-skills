---
name: descent-and-approach
description: Descend the Zibo 737-800X from cruise and set up an ILS approach: VNAV or LVL CHG descent, speed and drag, altimeter at the transition, pressurization, ILS tuning and identification, intercept from below, gear and flaps for a stabilised final. Use from top of descent until LOC and G/S are captured, or when you start within about 25 nm of the airport.
---

# Descent and approach

Read the `descent` and `approach` checklist sections and mark items in quiet moments. If you start close to the airport with the autopilot flying, look first (`assess-flight-state`) and do not change what is already right. Do not press `lnav` or `vnav` when the FMC has no route.

## Descent

1. Before the top of descent confirm the arrival and approach (chart and plan), then select the permitted lower altitude on the MCP. A VNAV descent cannot begin while the MCP stays at the cruise altitude. Watch the PROG `TO T/D`.
2. If VNAV does not descend (stale nav data, `NAV DATA OUT OF DATE`, no performance data), use `lvl-chg` (`fma-pitch` shows `MCP SPD`) or `v-s` deliberately with a verified altitude and speed, and manage drag with `speedbrake` value 2. Do not repeatedly press `altitude-intervention` or `speed-intervention`.
3. A short leg needs an early descent: check the path against the altitude you need at the final approach fix and be at about 160 kt before it. A 3.5 to 4 degree average path is often needed from cruise; LVL CHG alone is too shallow. Do not let the speed ride up.
4. Altimeter: `cockpit_altimeter` `std` at the transition level; at the transition altitude on descent `set-altimeter` with the QNH in inHg (`weather.qnh-in-hg`), then read `altimeter.setting` (the knob clicks, so it takes a moment).
5. Pressurization: read `landing-altitude` in `hydraulics-and-air` (feet) and compare it with the destination field elevation from the chart (the cabin altitude is a different reading). Correct it with one call of `set-landing-altitude` (destination elevation in feet, multiples of 50; it turns the knob itself, so do not press `landing-altitude-pressurization-up` / `-down` repeatedly) and read `landing-altitude` back. `flight-altitude` is the cruise altitude (500 ft steps).
6. Check the destination weather and choose a runway compatible with it, the available procedures and the autoland capability. Take the ILS frequency, identifier, magnetic course, intercept altitude, minima and missed approach from the chart; do not invent them.

## ILS setup

- Tune and identify with `cockpit_radios_and_ils`: `set-nav-1-standby-frequency` (MHz), `nav-1-swap`, `set-nav-1-course` to the inbound course; the same for Nav 2. Never write the radio datarefs. Verify `nav-1-frequency`, `nav-1-ident` (must match the chart), `nav-1-course`, then `nav-1-localizer`, `nav-1-glideslope` (dots), `nav-1-dme` and `nav-1-glideslope-flag`.
- A radio course may not take: the reading stays 0. Read it back; a zero course biases APP. `nav-1-dme` may not track distance: use the FMC PROG `DTG` or `latitude` / `longitude`.
- The localizer is receivable only on the approach side; an empty ident away from the final course is not a reason to scan frequencies.
- Set minimums, landing lights and signs, and the autobrake the checklist asks for.

## Intercept and configure

- Capture the glideslope from below: hold the altitude until the 3 degree path reaches it (about 320 ft of path per nautical mile) and do not descend by hand.
- Slow down early so that gear, landing flaps and landing speed are set before the glideslope. The autothrottle holds the selected speed: use `set-speed` in steps and extend the flaps within their limits. `speedbrake` value 2 for drag, value 1 (arm) for landing.
- Get onto an intercept heading 30 degrees or less from the localizer course, with the localizer alive (ident matches, `nav-1-localizer` within about two dots, on the final course), then press `app` once. Read `fma-roll-armed` `VOR/LOC` and `fma-pitch-armed` `G/S`. Do not assume autoland is available because `app` is selected.
- Continue with `final-and-landing` at LOC and G/S capture. If the approach is not stable by about 1,000 ft AGL or guidance is lost, `go-around`.

## RNAV approach instead

Follow the supported procedure: LNAV/VNAV final guidance shows `LNAV` and `VNAV PTH`; IAN uses `app` with `FAC` and `G/P`. Identify which this airplane supports first. An RNAV vertical path does not establish autoland: plan a manual landing.
