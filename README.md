# Anaglyph Asteroids

A 3D Asteroids clone rendered in red/cyan anaglyph stereo, built with
[three.js](https://threejs.org/). Classic Asteroids gameplay — rotate,
thrust, shoot, hyperspace — extended into a full 3D volume with toroidal
wraparound on all three axes.

**You'll want red/cyan 3D glasses (red lens on the LEFT eye).** The game
is playable without them, but the depth is the point.

## Running it

Open `index.html` in a browser. That's it — no build step, no dev server,
no install. The only external dependency is three.js, loaded from the
jsDelivr CDN via an import map, so you need an internet connection.

## Controls

| Input | Action |
|---|---|
| ← → ↑ ↓ or W A S D | Rotate the nose (yaw / pitch) |
| Mouse (click the game to capture it) | Steer the nose |
| Shift | Thrust along the facing direction |
| Right mouse button | Thrust |
| Space | Fire |
| Left mouse button | Fire |
| H | Hyperspace — random teleport, small chance of self-destruct |
| M | Mute / unmute |
| Enter (or click) | Start / restart |
| 1 / 2 / 3 | Difficulty preset on the title screen (easy / normal / hard) |

Keyboard and mouse work together — clicking the game grabs the pointer
(Esc releases it), and `ship.mouseSensitivity` in the config adjusts how
fast the mouse turns you.

Rotation is body-frame (relative to the ship itself), so there are no
gimbal dead spots and full 360° loops work on both axes. At spawn,
left/right rotates in the screen plane exactly like 2D Asteroids;
up/down pitches through the depth axis. Momentum is conserved — thrust
sets you drifting and only thrust (or hyperspace) changes that.

## Gameplay

- The playfield is a wireframe cube; fly out of one face and you re-enter
  the opposite one. Bullets, asteroids, and saucers all wrap the same way.
- Asteroids are Platonic solids (icosahedra, dodecahedra, octahedra,
  tetrahedra) in gem, wireframe, or gem+edge treatments. Shooting a large
  one splits it into two mediums, mediums into two smalls: 20 / 50 / 100
  points.
- Two saucer variants, warm-colored so they read as threats against the
  cool asteroid palette: the large one (200 pts) drifts and fires randomly;
  the small one (1000 pts) aims at you, and its aim tightens as your score
  grows.
- Clearing all asteroids advances the wave — more and faster rocks each
  time. Saucers spawn more often as your score climbs. Extra ship every
  10,000 points.
- Hyperspace re-enters at a random point with no safety check —
  materializing inside a rock is fatal, plus a configurable self-destruct
  chance on top, faithful to the original's gamble.

## Tuning

All gameplay numbers live in one place: the `DIFFICULTY_BASE` object near
the top of the script in `index.html` (ship handling, fire rate, wave
scaling, saucer spawn/aim ramps, scoring). The `DIFFICULTY_PRESETS` object
defines easy/normal/hard as sparse overrides that deep-merge onto the base.
If a run feels too easy or too hard, tune there — nothing is scattered
through the gameplay logic.

Non-difficulty knobs sit alongside it:

- `RENDER.stereo.eyeSep` — stereo strength. Raise for more pop, lower if
  your eyes complain.
- `ship.invertPitch` — flip W/S pitch direction if it feels backwards.
- `AUDIO.master` — overall volume. All sound is synthesized with WebAudio
  at runtime (no audio files), including the classic accelerating
  two-tone heartbeat.

## Implementation notes

Everything is in `index.html` on purpose: split ES modules can't load over
`file://`, and "double-click to play" was a design goal. The script is
organized into clearly banded sections (config, scene, input, ship,
asteroids, saucers, collisions, game flow) that mirror how separate
modules would have been cut.

Two rendering details worth knowing:

- The anaglyph pass is an inlined, lightly modified copy of three.js's
  `AnaglyphEffect` (MIT). The stock version hard-codes an eye separation
  of 0.064 world units — invisible at this scene's scale — so this copy
  exposes it as a tunable.
- The HUD (score, lives, wave, messages) is a plain DOM overlay, not part
  of the WebGL scene. Anything inside the stereo render double-images
  through the glasses; DOM text stays crisp.

`INIT.md` is the original design brief; `CLAUDE.md` holds the working
conventions used while building it.
