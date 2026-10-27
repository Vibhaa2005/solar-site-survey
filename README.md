# Solar Site Survey

A phone web app that measures how well a rooftop solar panel is placed, using the phone's own sensors:

| Sensor | What it measures |
|---|---|
| Accelerometer (gravity direction) | Panel tilt from horizontal |
| Magnetometer + gyroscope (fused orientation) | Direction the panel faces; direction the camera points |
| GPS | Latitude, which sets the sun's path |
| Camera | Viewfinder for tracing the skyline |

**Align tab:** lay the phone flat on the panel. It shows the live tilt and direction, the best tilt and direction for your latitude, a yield map (a polar heat map of annual output for every tilt and direction), the % energy lost, and kWh/₹ lost per year.

**Shade tab:** point the camera at the skyline and turn through 360°. The app records how high the obstacles are in each direction, draws them on a sun-path diagram, and calculates the hours of direct sun and the energy lost to shade, month by month.

**Report tab:** alignment and shading combined into one "% of ideal" figure, expected kWh/year, money lost, and recommendations. Exports a CSV.

All the code is in `index.html`: plain HTML/CSS/JavaScript, no libraries, no build step.

## Hosting (needed for sensors)

Phone browsers only give sensor, GPS and camera access to pages served over **https**. Opening the file directly won't work on a phone. The easiest free option is GitHub Pages:

1. Create a public repo on github.com (e.g. `solar-site-survey`) and upload `index.html` (Add file → Upload files).
2. Settings → Pages → Source: *Deploy from a branch*, Branch: `main`, folder `/ (root)` → Save.
3. After about a minute the app is live at `https://<username>.github.io/solar-site-survey/`.

Works best in **Chrome on Android**. On iPhone (Safari), tap Start sensors and allow Motion & Orientation. Lay the phone flat for 2 s first so the compass locks.

## Method (short)

- Panel normal = third column of the rotation matrix Rz(α)·Rx(β)·Ry(γ) from the phone's orientation; tilt = acos(n_up), azimuth = atan2(n_east, n_north).
- Sun position from declination and hour angle, every 3rd day at 10-minute steps.
- Clear-sky irradiance: Kasten–Young air mass, Meinel beam model, 10% diffuse.
- Plane-of-array irradiance from the isotropic-sky model. Beam light counts only when the sun is above the measured skyline; diffuse is scaled by the sky-view factor.
- kWh/yr = kWp × annual irradiation × performance ratio × 0.74 (cloud/haze factor calibrated to typical Indian yields of about 1,500 kWh/kWp). The percentages don't depend on this factor.
