# DRIVOMATE ADAS

Smartphone-based rider assistance prototype. Vehicle and pedestrian detection runs
on-device with **YOLO11n** (Ultralytics, COCO-80) via onnxruntime-web.

    index.html              the whole app (242 KB) — UI, risk engine, real field data
    models/yolo11n.onnx     detection weights (10.2 MB), fetched at runtime
    .nojekyll               serve files verbatim, no Jekyll processing

`index.html` loads `models/yolo11n.onnx` by **relative path**, so the `models/` folder must
stay next to it. Renaming or flattening the folder breaks detection — the Status screen will
say so explicitly rather than failing silently.

## GitHub Pages

    git init
    git add index.html models/yolo11n.onnx .nojekyll README.md
    git commit -m "DrivoMate ADAS"
    git branch -M main
    git remote add origin https://github.com/<you>/<repo>.git
    git push -u origin main

Then **Settings → Pages → Deploy from a branch → main / (root)**, wait for the green check,
and open `https://<you>.github.io/<repo>/`.

The model is 10.2 MB — fine for Pages (100 MB/file limit) and for a normal `git push`, no
Git LFS needed.

## Why it must be served over HTTPS

Camera, GPS and motion sensors only exist in a **secure context**. GitHub Pages is HTTPS, so
they work there. Opening this from `file://` silently denies all three — a browser rule, not
an app bug. For local testing use `python3 -m http.server 8000` and `http://localhost:8000/`.

## Network use

- **onnxruntime-web** (~3 MB) from jsDelivr, once, then browser-cached. The model itself is
  local. Without the runtime, detection is unavailable and the app falls back to GPS and
  motion alerts only.
- **Esri** dark basemap tiles, for the Map and Danger Zones screens.
- **Overpass API**, for road names and speed limits.

## Notes

- Detection classes used: person, bicycle, car, motorcycle, bus, truck, plus a few animal
  and object classes treated as obstacles. Potholes and debris are NOT detected.
- Vehicle warnings additionally require the vehicle to be in your path and require rider
  motion, so standing still and pointing the phone at a car is correctly silent. The
  "Tracking" line in the live view shows what the detector sees regardless of warnings.
- Experimental prototype. Not a certified safety system.
