# Solar Site Survey

A phone web app that measures how well a rooftop solar panel is placed, using the phone's own sensors:

| Sensor | What it measures |
|---|---|
| Accelerometer (gravity direction) | Panel tilt from horizontal |
| Magnetometer + gyroscope (fused orientation) | Direction the panel faces; direction the camera points |
| GPS | Latitude, which sets the sun's path |
| Camera | Viewfinder for tracing the skyline |

**Live app:** https://vibhaa2005.github.io/solar-site-survey/

**Step 1 · Angle** (phone flat on the panel): score as % of the best angle; a Yours / Best / Change table; kWh and ₹ lost per year; two "what if" line charts (energy vs tilt and energy vs direction, with the within-1% band); a polar map of every possible angle; live raw sensor readings and a tilt-over-time trace.

**Step 2 · Shade** (camera pointed at the skyline, turning 360°): % of yearly energy lost to shade; direct-sun hours, main obstacle and worst months; a sun-path diagram with the traced skyline (blocked sun in red); direct-sun hours by month.

**Step 3 · Result:** overall % of ideal with a one-line verdict; a Now / Angle fixed / Both fixed comparison (kWh, ₹ per year, ₹ over 25 years, CO₂); power through a clear summer, equinox or winter day showing when energy is lost; where the year's energy goes; actions ranked by gain; survey details and CSV export.

All the code is in `index.html`: plain HTML/CSS/JavaScript, no libraries, no build step.

## Hosting

Phone browsers only give sensor, GPS and camera access to pages served over **https**, so the app is hosted on GitHub Pages (link above). Works best in **Chrome on Android**. On iPhone (Safari), tap Start sensors and allow Motion & Orientation. Lay the phone flat for 2 s first so the compass locks.

## Method (short)

- Panel normal = third column of the rotation matrix Rz(α)·Rx(β)·Ry(γ) from the phone's orientation; tilt = acos(n_up), azimuth = atan2(n_east, n_north).
- Sun position from declination and hour angle, every 3rd day at 10-minute steps.
- Clear-sky irradiance: Kasten–Young air mass, Meinel beam model, 10% diffuse.
- Plane-of-array irradiance from the isotropic-sky model. Beam light counts only when the sun is above the measured skyline; diffuse is scaled by the sky-view factor.
- One baseline everywhere: 100% = the open-sky optimum. The "best angle" is the best angle for this spot including the traced skyline. **Angle loss** = best angle − your panel (both with shade); **shade loss** = open-sky optimum − best angle. The two always add up to the total loss, so every tab shows the same numbers.
- Measurement quality: tilt and direction are the mean of the last 5 s of raw readings, reported with their spread (standard deviation; circular SD for direction).
- Payback = one-time fix cost (editable estimate in Setup) ÷ yearly saving.
- kWh/yr = kWp × annual irradiation × performance ratio × 0.74 (cloud/haze factor calibrated to typical Indian yields of about 1,500 kWh/kWp). The percentages don't depend on this factor.
