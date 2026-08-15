# Selfie Quality Check

A static, browser-only selfie quality gate designed for low-end and older Android phones.

## Checks

- Exactly one face is visible
- The face is close enough and fully inside the frame
- The face is looking approximately straight ahead (yaw and pitch heuristic)
- The face crop has acceptable sharpness
- The face crop is not severely under- or overexposed

All camera processing happens locally. The page has no backend, uploads, analytics, custom classifier, or build step.

## Run locally

Camera access requires a secure context. Serve the repository over HTTPS or use a local development server:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Test blur and head angle

Append `?debug=1` to the page URL to reveal local test controls and live values for sharpness, brightness, yaw, and pitch. The **Simulate blur** switch blurs only the 128×128 analysis crop, so it tests the quality gate without changing or uploading the camera stream.

Append `?debug=blur` to start with simulated blur already enabled. For production, use:

```text
https://m-sudo011.github.io/tapsi-selfie-test/?debug=blur
```

Turn your head toward a three-quarter/profile view or look clearly up/down to test the angle feedback. The production URL without a `debug` parameter does not show the test controls.

## Tuning

Quality and distance thresholds are centralized in the `CONFIG` object near the top of the JavaScript in `index.html`.

The initial values are deliberately conservative starting points. Validate them against representative target devices and conditions, especially:

- older Android front cameras
- low indoor light
- motion blur
- front, three-quarter, profile, up, and down head poses
- different face sizes and positions
- a second face near the edge or background

The `minSharpness` value uses variance of a four-neighbor Laplacian over a standardized 128×128 grayscale face crop. It should be calibrated from labeled sharp and blurred samples captured on target phones.

The head-angle gate intentionally reuses BlazeFace's six existing landmarks. `maxYawRatio`, `minPitchRatio`, and `maxPitchRatio` are normalized heuristics rather than degree measurements. This keeps the page light, but the thresholds should be calibrated on representative faces and phones. If precise pose angles become a requirement, use a denser face-landmark model instead.

## Production

GitHub Pages publishes the root of `main` automatically.
