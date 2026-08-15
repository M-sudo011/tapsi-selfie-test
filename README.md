# Selfie Quality Check

A static, browser-only selfie quality gate designed for low-end and older Android phones.

## Checks

- Exactly one face is visible
- The face is close enough and fully inside the frame
- The face crop has acceptable sharpness
- The face crop is not severely under- or overexposed

All camera processing happens locally. The page has no backend, uploads, analytics, custom classifier, or build step.

## Run locally

Camera access requires a secure context. Serve the repository over HTTPS or use a local development server:

```sh
python3 -m http.server 8000
```

Then open `http://localhost:8000`.

## Tuning

Quality and distance thresholds are centralized in the `CONFIG` object near the top of the JavaScript in `index.html`.

The initial values are deliberately conservative starting points. Validate them against representative target devices and conditions, especially:

- older Android front cameras
- low indoor light
- motion blur
- different face sizes and positions
- a second face near the edge or background

The `minSharpness` value uses variance of a four-neighbor Laplacian over a standardized 128×128 grayscale face crop. It should be calibrated from labeled sharp and blurred samples captured on target phones.

## Production

GitHub Pages publishes the root of `main` automatically.
