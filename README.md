# Plasma Disc

A giant, web-based plasma globe. Runs in any browser, scales to fill any screen (great for casting to a TV), reacts to your touch/cursor the way a real plasma disc's arcs bend toward a fingertip, and can pulse along with music playing in the room via the microphone.

**[Live demo](https://michael714.github.io/plasma-disc/)**

## Features

- Three concentric color bands — green core, red/orange middle, blue rim — matching a real plasma disc
- Constantly re-branching "lightning" arcs drawn with recursive midpoint displacement, so they flicker and crackle like real plasma filaments
- Move your cursor or touch the screen near the glass and the nearest arcs bend to reach it, just like touching a real globe
- Tap **Plasma Buzz** to hear the synthesized hum and crackle; **Microphone** makes the glow pulse with music in the room, or **Play a song** to load a local audio file
- Tap **Fullscreen** for a clean, edge-to-edge picture — ideal when casting to a TV or large display
- Pure HTML/CSS/JS, no build step, no dependencies — a single `index.html` file

## Running locally

Just open `index.html` in a browser, or serve the folder so the microphone permission prompt behaves consistently:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying

This repo is set up to deploy automatically to GitHub Pages via GitHub Actions on every push to `main` (see `.github/workflows/deploy.yml`). See `CURSOR_PROMPT.md` for a ready-to-run prompt that walks Cursor through publishing it.

## Browser notes

- Microphone-based sound reactivity requires HTTPS (GitHub Pages serves over HTTPS automatically) and an explicit permission grant — Safari and iOS in particular need a user tap before audio can start.
- Best viewed in a Chromium or Safari browser with hardware acceleration enabled; very large TVs may want a dedicated streaming device (Chromecast, AirPlay, a mini-PC) running the browser rather than the TV's own low-powered browser.
