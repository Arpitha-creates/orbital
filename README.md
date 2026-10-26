# Orbital

A 3,000-particle sphere you can hold in your hand — moved, rotated, charged and
shattered in real time through your webcam.

One file. No frameworks, no build step, no bundler. The particles are drawn with
raw WebGL; hand tracking comes from MediaPipe Hands loaded from a CDN.

## Gestures

| Gesture | What happens |
| --- | --- |
| **One hand** | The sphere follows your palm and spins with the momentum of your movement |
| **Pinch and hold** | A ring fills at your fingertips while the sphere contracts; release to throw the particles across the screen, then watch them spring back into a sphere |
| **Two hands, spread apart** | The sphere grows |
| **Two hands, brought together** | The sphere shrinks |
| **Finger count (0–5)** | Changes the particle colour |

## Run it

The webcam needs a secure context, so `file://` will not work in most browsers.
Serve the folder over `localhost`, or use the GitHub Pages link:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Click **Enable camera** and allow access. If you would rather not use the camera,
**Use pointer** gives you the same behaviour with the mouse: move to orbit, hold
to charge, release to shatter.

## Controls

| Key | Action |
| --- | --- |
| `C` | Toggle the webcam as a fullscreen background |
| `Space` | Burst the particles without the camera |

The button in the bottom right toggles the webcam background too.

## How it works

- 3,000 particles are distributed over a Fibonacci sphere, so they sit evenly
  with no clustering at the poles.
- Each particle is a point sprite with a soft radial falloff, blended additively
  for a glow rather than hard dots.
- Particles are driven by a spring toward their target positions, which is what
  gives the burst its return and slight overshoot.
- A finger counts as extended when it is straight at the knuckle *and* its tip
  sits further from the wrist than the knuckle does. Both tests are angles and
  distances rather than screen directions, so counting still works with the hand
  tilted.
- Frames are fed to the tracker one at a time, only after the previous result
  returns, and downscaled first — full-resolution frames queued faster than the
  model can answer will stall the page on slower machines.
- If the device cannot hold a comfortable frame rate, the render resolution is
  stepped down once instead of stuttering.

## Privacy

Video never leaves the device. Tracking runs entirely in the browser; nothing is
uploaded or recorded.

## Licence

MIT
