# Camera Recorder

A browser-based camera and microphone recorder with optional RNNoise noise reduction and PWA support. Designed to capture camera video and system or microphone audio, preview recordings, and automatically download WebM files.

## Features
- Record camera (select device) and mix with system or microphone audio.
- Optional RNNoise noise filtering for microphone input.
- Live preview during recording and automatic WebM download when recording stops.
- PWA-ready (manifest, icons, service worker support).

## Files of interest
- `www/index.html` — main UI and recording logic (camera selection, RNNoise toggle, record/pause/stop).
- `www/manifest.json` — PWA manifest with icon references.
- `www/icons/` — icon files used by the manifest (use a maskable SVG for best install results).
- `libs/rnnoise.js` — RNNoise WebAssembly wrapper used for noise reduction.
- `server.js` — optional Node static server at project root.

## Quick start (local)
Serve the `www` folder for full PWA and service-worker behavior. From the project root:

```bash
# using Python 3 built-in server
python3 -m http.server 8000 --directory www

# or using Node (http-server)
npx http-server www -p 8000
```

Open `http://localhost:8000` in a modern browser.

## Usage
1. Select the camera from the `Select Camera` dropdown.
2. Choose audio source: `Microphone` or `System Audio`.
3. (Optional) Enable `Enable Noise Filtering (RNNoise)` to reduce microphone background noise.
4. Click `Record`. Use `Pause` and `Stop` as needed. When stopped, the recording will be downloaded automatically as a `.webm` file.

## PWA / Icons
- To support maskable install icons, add a maskable SVG at `www/icons/icon.svg` and include this manifest entry:

```json
{
  "src": "icons/icon.svg",
  "sizes": "any",
  "type": "image/svg+xml",
  "purpose": "any maskable"
}
```

- Keep PNG fallbacks (72/96/128/144/152/192/384/512) in the manifest for maximum compatibility.

## Generate PNGs from SVG
If you have ImageMagick:

```bash
convert -background none www/icons/icon.svg -resize 512x512 www/icons/icon-512.png
```

Or using `rsvg-convert`:

```bash
rsvg-convert -w 512 -h 512 www/icons/icon.svg -o www/icons/icon-512.png
```

## Troubleshooting
- Some browsers require HTTPS or localhost to capture system audio via `getDisplayMedia` — use a local server or HTTPS.
- If camera or microphone labels are empty, grant permissions and reload; the UI requests temporary permissions to enumerate devices.
- RNNoise may not initialize on all platforms; the UI falls back to unfiltered audio if RNNoise fails.

## License
This project is licensed under the Apache 2.0 License (see `LICENSE`).
