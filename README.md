<!-- #dev/creator-tubakhxn -->
# Jet Engine — Gesture-Controlled 3D Explorer

A full-screen, black-and-white wireframe **exploded-view jet engine** that you control with your **bare hand through your webcam**. Built with Three.js and MediaPipe Hand Landmarker, running entirely in the browser: no backend, no build step, no Python or GPU setup.

Creator: **#dev/creator-tubakhxn**

## What it does

- Renders a procedural 3D jet engine (fan section, core, nozzle) as a white CAD-style wireframe, with blue and yellow-green accent rings, axes and technical labels.
- Tracks your hand in real time and shows fingertip circles in the webcam preview.
- Lets you select engine sections, spin the end parts, compress the whole engine and orbit it, all with hand gestures.

## Gestures

| Gesture | Result |
|---|---|
| Open palm | Nozzle selected ("exhaust/nozzle"); end sections spin up |
| Two fingers (index + middle) | Core selected (compressor / combustion / turbine) |
| Three fingers (index + middle + ring) | Fan section selected |
| Fist | Engine compresses into one assembled piece |
| Move your hand | Engine orbits (left/right, up/down) |
| Tilt / rotate your hand | Engine rolls |

The thumb is ignored. Gestures must hold ~0.2 s to trigger, and the last mode is kept if your hand leaves the frame. The text under the camera preview shows live finger states (`1` up / `0` down for index, middle, ring, pinky).

## Keyboard fallback

| Key | Action |
|---|---|
| `1` / `2` / `3` | Nozzle / Core / Fan |
| `4` or `C` | Compress |
| `R` | Reset view |
| `H` | Show / hide webcam preview |
| Mouse drag | Orbit |

The 3D scene and keyboard controls work even if the camera or hand model is unavailable.

## Project structure

```
jet-engine-gesture/
├── index.html                    page layout, import map
├── style.css                     styling and layout
├── main.js                       3D engine, labels, gestures, hand tracking
├── vendor/three.module.min.js    Three.js r160 (bundled locally)
└── README.md
```

## Run it locally

Webcam access only works on `http://localhost` or `https://`, so do not open `index.html` by double-clicking. Start a tiny local server inside the project folder:

```bash
# Python (already on most machines)
python -m http.server 8000        # Windows alternative: py -m http.server 8000

# or Node.js
npx serve .
```

Open **http://localhost:8000** in Chrome or Edge and click **Allow** for the camera. Press `Ctrl + C` in the terminal to stop.

First run needs internet once: the hand model (~8 MB, from Google's MediaPipe storage) and the MediaPipe library (jsDelivr CDN) are downloaded and then cached by the browser. Fonts come from Google Fonts, with system fallbacks.

## Fork and make it yours

1. Click **Fork** on the GitHub repository page (or create a new repo and push this folder).
2. Clone your copy:
   ```bash
   git clone https://github.com/<your-username>/jet-engine-gesture.git
   cd jet-engine-gesture
   ```
3. Run it with one of the commands above and edit away.
4. Commit and push:
   ```bash
   git add .
   git commit -m "My changes"
   git push
   ```

### Deploy for free (static site)

- **GitHub Pages:** repo Settings, Pages, deploy from the `main` branch, root folder.
- **Vercel / Netlify:** import the repo; no build command, output directory is the project root.

Both serve over HTTPS, so the webcam works on the live site.

## Customize

All in `main.js`:

| What | Where |
|---|---|
| Gesture sensitivity | `UP_RATIO`, `DOWN_RATIO` (finger up/down thresholds) |
| Gesture hold time | `HOLD_MS` |
| Exploded / compressed spacing | `EXPLODED_X`, `ASSEMBLED_X` |
| Engine shapes | `buildFan`, `buildCore`, `buildNozzle` |
| Hand-orbit strength and follow speed | `updateHandPose` and the follow rate in `frame` |
| Colors | `BLUE`, `LIME`, and the title gradient in `style.css` |

## Privacy

Camera frames are processed locally in your browser. Nothing is uploaded, saved or recorded.

## Limitations

- The engine is a procedural model, not engineering-grade CAD.
- Gesture accuracy depends on lighting and keeping your hand in view.
- Hand tracking needs the one-time model download; offline, use the keyboard.

## Credits

- [Three.js](https://threejs.org) (MIT)
- [MediaPipe Tasks Vision](https://developers.google.com/mediapipe) Hand Landmarker (Apache 2.0)
- Fonts: Orbitron, Share Tech Mono, Space Mono (Google Fonts, OFL)
