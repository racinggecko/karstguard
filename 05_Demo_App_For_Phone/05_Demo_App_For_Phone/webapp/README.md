# KarstGuard PA — field demonstration build

A single-file build of the app that runs on a phone today, used to film the
demonstration video while the Flutter client in `../app/` is completed.

It is not a mock-up. Every number on the capture screen is measured from the
live camera feed:

| On screen | How it is produced |
| --- | --- |
| Sharpness | Variance of the Laplacian over the frame |
| Marker / Lock | Largest near-square dark region, found by flood fill |
| Camera angle | `acos` of the marker's side ratio — a square photographed off-axis stops being square |
| Alignment quality | The **weakest** of the three checks, never the average |
| Calibration (mm/px) | 50 mm marker width ÷ its width in pixels |
| Measured change | Median darkest contiguous run through the region of interest, in mm |

The gate refuses to enable the shutter until all three checks pass, exactly as
`quality_gate.py` does on the server.

## Installing on the Android phone

1. Commit this folder and enable **GitHub Pages** on the repository
   (Settings → Pages → deploy from branch). HTTPS is required — the camera API
   will not start over plain HTTP.
2. Open `https://<user>.github.io/<repo>/webapp/` in Chrome on the phone.
3. Menu (⋮) → **Add to Home screen**. It installs with the KarstGuard icon and
   launches full screen with no browser chrome, so it films like a native app.
4. First launch will ask for camera permission. Grant it **before** filming.

## Filming the capture sequence

The first capture stores a baseline; the second measures change against it.
For a demonstration you want the second one on camera.

1. Print `../marker/KarstGuard_Marker_PrintSheet.pdf` and check the 100 mm bar.
2. At the crack, open the app → **Guided road scan** → capture once. That is
   the baseline. Stop recording.
3. Start recording. Capture again. The app now shows a real measured delta and
   moves to Vision analysis on its own.

To reset between takes, **triple-tap the header**. That clears the stored
baseline so the next capture is a fresh first scan.

## Getting a deliberate rejection on camera

The rejection is the most persuasive beat in the video, and it is easy to
trigger honestly:

- **Angle** — tilt the phone until the marker skews. The banner names the
  measured angle and the tolerance.
- **Sharpness** — move the phone while capturing.
- **Marker** — cover the marker with a hand.

Then correct it and let the banner turn green before capturing. That whole
sequence is real; nothing is scripted or on a timer.

## Filming notes

- Overcast light or open shade. Direct sun puts a specular hotspot on the
  marker and detection drops out.
- Keep the marker flat and fully inside the frame, roughly 50–150 mm from the
  crack and in the same plane as the road surface.
- Record the phone screen at the same time (Android built-in screen recorder)
  if you want a clean UI capture to inter-cut with the over-the-shoulder shot.
- Portrait, 1080×1920. The composited clips place the device in the right third
  of a 16:9 frame, so portrait footage drops straight in.

## What this build does not include

`My area` shows a static illustrative map, and the priority signals on the
packet screen are the fixed published weights (34 / 16 / 12 / 10). Live map
tiles and server-side scoring belong to the Flutter client and the FastAPI
service, and those screens stay as composited clips in the video.
