# PURNANSH ADAS

Single-file smartphone ADAS prototype. Open `index.html` — everything (CSS, JS, and the
on-device detection model) is embedded, so there is nothing to build and no dependencies
to install.

## Publishing on GitHub Pages

    git init
    git add index.html .nojekyll README.md
    git commit -m "Purnansh ADAS prototype"
    git branch -M main
    git remote add origin https://github.com/<you>/<repo>.git
    git push -u origin main

Then: repo -> **Settings -> Pages -> Source: Deploy from a branch -> main / (root) -> Save**.
Wait for the green check on the Pages deployment, then open
`https://<you>.github.io/<repo>/`.

The file MUST be named `index.html` for that root URL to work. If you keep the original
name instead, the URL is `https://<you>.github.io/<repo>/purnansh-adas.html`.

`.nojekyll` tells Pages to serve files verbatim rather than running them through Jekyll.

## Why it must be served over HTTPS

The camera, GPS and motion sensors are only available in a **secure context**. GitHub Pages
serves over HTTPS, so permissions work there. Opening the same file from `file://` silently
denies all three — that is a browser rule, not a bug in the app.

## Notes

- The base map uses CARTO's dark tiles (Esri fallback). It does not use
  `tile.openstreetmap.org`, whose usage policy blocks unidentified clients with HTTP 403.
- Road names and speed limits come from the Overpass API and need network access.
- This is an experimental prototype, not a certified safety system.
