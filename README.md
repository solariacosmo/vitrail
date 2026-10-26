# Vitrail

A Gothic nave lit only by its stained-glass windows. Shafts of coloured light cross the haze, land on the stone and pool on a polished floor. Everything runs in real time in the browser.

**[Open it →](https://solariacosmo.github.io/vitrail/)**

![The five hours, from pre-dawn to night](screens/hours.jpg)

## The hours

The light follows your local clock by default (**Now**), blending between five canonical hours as the day goes on. The slider in the control bar runs across a whole day and is tinted like the light. Drag it, or tap anywhere on it, to visit another time of day, and tap **Now** to go back to the clock. The arrow keys move it in 15-minute steps.

| Hour | Time | Light |
|---|---|---|
| **Matins** | pre-dawn | cool lavender light, low from the left |
| **Lauds** | sunrise | warm, low sun through the left-hand windows |
| **Sext** | noon | high sun; patches of colour fall in the middle of the floor |
| **Vespers** | sunset | amber light raking in from the right |
| **Compline** | night | moonlight through the great west window |

**Terracotta glass** swaps every window to a warm clay-orange palette with ivory, oat and sand panes.

## Sound

Tap **Sound** to hear the nave. Everything is synthesized in the browser with the Web Audio API. There are no audio files.

- **Drone**: an organ-like drone whose mode and chords follow the hour. It is Dorian before dawn, Mixolydian at sunrise, open fifths at noon, a warm major at sunset and a low Aeolian at night.
- **Chimes**: sparse glass chimes, more often by day and rarer at night.
- **Hour bell**: a distant bell tolls the hour, from one to twelve strikes, and the drone dips a little while it speaks. It has its own toggle, and turning it on plays a single strike so you know what to listen for.
- **Reverb**: a generated stone-hall reverb with a six-second tail.

The mix is centred and low-passed, so nothing is shrill and nothing jumps between the left and right speakers.

### Listening in the background

The sound keeps playing when you switch to another app or lock the screen, like a music app, and it shows up in the lock-screen media controls. To make that possible, the page renders the current hour's drone and chimes into a seamless loop of four to six minutes (this takes a few seconds the first time), writes it out into a 60-minute track whose first seconds carry the hour bell, and plays that through an ordinary `<audio>` element.

The track is started at the position that matches the clock, so if it is 21:40 it starts 40 minutes in. It then comes back round to its beginning, and rings the bell, exactly on the hour, with or without the page on screen. Near each hour the page prepares the next hour's track (with one more strike) and swaps to it on the hour. While the page is open, a change of light crossfades into the next hour's music.

On iPhone and iPad, Safari is the most reliable for background playback. Some other browsers on iOS stop all sound when they go to the background, and a page added to the home screen may not get to swap tracks while it is in the background, in which case the bell repeats the previous count until you open it again.

## Controls

- **Drag** anywhere to turn the sun. Double-click resets it.
- **Tap** the scene to hide or show the controls.
- **Gyroscope** (phones and tablets): tap the button and allow motion access. Then tilt left and right to swing the sun, or forward and back to raise and lower it. iOS only grants motion access to pages served over HTTPS, which GitHub Pages is.
- On iPhone or iPad, **Share → Add to Home Screen** opens it full screen like an app.

## How it works

It is a single `index.html` with no build step, no libraries and no third-party requests, written in WebGL2.

- **Geometry**: a signed-distance-field nave with a pointed barrel vault, transverse ribs, clustered piers, lancet window recesses and a great west window with tracery and a rose. Rays are sphere-traced against it.
- **Glass**: every window is painted procedurally into a 4096 × 2048 canvas atlas. That includes the quarries, medallions, lead came, saddle bars and stone tracery, plus some streaky mottling.
- **Light through glass**: for any point in the room, the sun's visibility is found analytically. The point is projected along the sun direction onto the window planes, then the atlas is sampled there. The mip level grows with distance, so projected patterns soften the way real ones do.
- **Haze**: a half-resolution volumetric pass marches through tileable 3D noise fog with a Henyey–Greenstein phase function. That pass also lights the haze seen in floor reflections. It is upsampled with a small tent filter.
- **Floor**: polished checkerboard marble with Fresnel reflections and a slight ripple.
- **Finish**: dust motes that only show inside the beams, bloom built from the mip chain, ACES tone mapping, and dynamic resolution that keeps the frame rate smooth.
- **Motion**: the light drifts slowly and nothing flashes. With *reduce motion* turned on, the drift starts paused.

## Running locally

Open `index.html` in any browser that supports WebGL2. The gyroscope needs the page to be served over HTTPS.

## Licence

MIT. See [LICENSE](LICENSE).
