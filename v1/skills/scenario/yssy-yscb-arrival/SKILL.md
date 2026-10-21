---
name: yssy-yscb-arrival
description: Land the Zibo 737-800X at Canberra YSCB from a start 25 nm out on the runway 35 centreline at 5000 ft and 220 kt, with the autopilot holding heading, altitude and speed and no route in the FMC. Use when the scenario starts on the approach and only the approach and landing are left.
---

# Sydney to Canberra, 25 nm final

Use the `fly-the-airplane` skill for how the tools work, the `ils-final-hands-off` skill once LOC and G/S are captured, and the `landing` skill for the autoland setup and the rollout. The chart numbers for Canberra ILS Z RWY 35 and the go-around rules are in the `yssy-yscb` skill (section "ILS final approach and autoland" and "Failures and go-around"); select both skills when queueing.

## Where you start

The airplane is already in the air. irl-gym set it up like this; read it, do not assume it:

- On the extended centreline of YSCB runway 35, about 25 nm from the threshold, heading 348 (magnetic), 5000 ft, 220 kt.
- Engines running, gear up, flaps 0, fuel about 3.4 t.
- CMD A on and CMD B off, both flight directors on, HDG SEL (348) and ALT HLD (5000) active, autothrottle armed in SPEED at 220 kt.
- LNAV, VNAV, VOR/LOC and APP are off. The FMC has no route.

Nothing else has been prepared: altimeter, lights and signs, EFIS, minimums, radios and autobrake are yours. The phase is tracked automatically; the run covers the descent, approach, landing and after landing sections of the checklist.

## 1. Look before touching

`get_status`, then `read_group` for `mcp-and-autopilot` (the FMA readings `fma-roll`, `fma-pitch`, `fma-autothrottle`, both CMD lights and the selected altitude, speed and heading), `flight-instruments`, `altimeter`, `radios-and-ils`, `brakes-flaps-and-gear` and `weather`. Do not change anything that is already right. Do not press `lnav` or `vnav`: there is no route to fly.

## 2. Prepare the approach

1. Read the `descent` and `approach` checklist sections and work through them, marking what you did and confirmed.
2. Set the altimeter: `cockpit_altimeter` `set-altimeter` with the QNH in inHg (`weather.qnh-in-hg`), and check `altimeter.setting`.
3. Tune and identify the ILS as in `yssy-yscb`: 109.5 MHz in Nav 1 and Nav 2, course 348. The localizer is received on the approach side, so the ident may stay empty until you are within range; do not scan frequencies. Read the course back (`nav-1-course`): it may not set the first time.
4. Set minimums, the landing lights and signs, and the autobrake the checklist asks for.
5. You may enter the arrival in the FMC for reference (PROG distance to go), but fly the approach on the ILS, not on LNAV or VNAV.

## 3. Intercept and configure

- At 5000 ft level, you capture the glideslope **from below** at about 10 DME from ICB (the 3° path is 5050 ft at 10 DME). Keep ALT HLD at 5000 until then and do not descend by hand.
- You are at 220 kt; start slowing early so that gear, landing flaps and the landing speed are set before the glideslope. The autothrottle holds the selected speed, so slow down with `set-speed` in steps and extend flaps within their speed limits (see `landing`). Use `speedbrake` value 2 for drag if needed, and arm it (value 1) for landing.
- Distance: `nav-1-dme` may not track here; cross-check with `latitude` / `longitude`, `ground-speed` and the localizer dots.
- Arm `app` once the localizer is alive (ident ICB, `nav-1-localizer` within about two dots, the airplane on the 348 course). Verify LOC and G/S **armed** (`fma-roll-armed`, `fma-pitch-armed`), then **captured** (`fma-roll` `VOR/LOC`, `fma-pitch` `G/S`). From that moment follow `ils-final-hands-off`: one `cmd-b` before 1500 ft AGL and then hands off.

## 4. Landing

Follow `landing` and the minima and missed approach in `yssy-yscb`. If the approach is not stable by about 1000 ft AGL, or guidance is lost, go around (`to-ga-left`), as in `yssy-yscb`. After touchdown: speedbrakes, reverse thrust and brakes, autopilot disconnect once slow and straight, and vacate the runway as in `landing`.
