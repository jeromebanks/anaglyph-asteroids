# 3D Anaglyph Asteroids — Build Brief

Build a 3D Asteroids game in three.js, rendered in anaglyph 3D (red/cyan).
Single-player, browser-based, no build step required if reasonably avoidable
(plain HTML/JS/three.js via CDN or local module is fine).

## Camera & Playfield

- Fixed camera outside a cubic bounding volume, slight perspective tilt so
  depth is readable in stereo. No chase-cam, no flight-sim orientation —
  this should read as classic Asteroids extended into a depth axis, not a
  cockpit sim.
- Toroidal wraparound on all three axes (x, y, z): exit one face of the
  cube, re-enter the opposite face, matching 2D Asteroids screen-wrap.

## Ship & Controls

- Ship translates freely on all 3 axes (strafe-style thrust, not 6DOF
  pitch/roll/yaw flight controls) and rotates/banks to face its direction
  of travel.
- Shoots along its facing vector.
- Use your judgment on control scheme (keyboard, or keyboard+mouse) —
  optimize for readability in a fixed-camera 3D space.

## Physics

- No need for a full rigid-body physics engine — classic Asteroids has no
  gravity/friction, just momentum-conserving velocity integration and
  collision response. Use your judgment on whether a lightweight physics
  library or hand-rolled vector math better serves this; don't over-engineer.

## Asteroids

- Replace rocks with Platonic solids and tessellated/subdivided polyhedra
  (icosahedra, dodecahedra, etc. at varying detail levels). Pure geometric
  aesthetic — no jagged noise-deformed rock look.
- Be creative here: vary geometry type, subdivision level, size, slow
  tumble rotation, and color/material treatment per-asteroid for visual
  variety, within a clean geometric-abstraction style (think low-poly /
  wireframe / gem-like rather than "space rock").
- Splitting-on-hit mechanic from classic Asteroids (large → medium → small)
  translated to this shape system.

## Alien Saucers

- Two variants, mirroring the original: large saucer (slow drift, fires
  in random directions, low threat/low points) and small saucer (faster,
  aims toward the player with some inaccuracy that tightens as score
  increases, higher points).
- Spawn at random points on the playfield boundary and drift inward on
  a wander/patrol path; participate in the same toroidal wraparound as
  everything else. Spawn rate/frequency should scale with score or time
  survived (see Difficulty below).
- Visually distinct from the asteroid geometry family at a glance — use
  your judgment on shape (e.g. a shape family not used for asteroids,
  like a stellated or toroidal form) and material/color treatment so
  saucers read immediately as threats, not more debris.
- Saucers are destructible (player shots) and can destroy the player
  (collision or their own shots); integrate into existing scoring.

## Progression & Difficulty

- Gameplay progression should mirror the original Asteroids: a level
  starts with a handful of large asteroids, clearing a level (destroying
  all asteroid fragments) advances to the next wave with more/faster
  asteroids, saucer spawn frequency increases with score/survival time.
- **Make all of this configurable rather than hardcoded** — asteroid
  count per wave, asteroid speed/scaling per wave, saucer spawn interval
  and accuracy ramp, player fire rate, lives, etc. should live in a single
  tunable difficulty config (e.g. a `DIFFICULTY` object or a small JSON/JS
  settings module), not scattered magic numbers. This is so it can be
  tuned after playtesting if it turns out too easy or too hard.
- A few named presets (e.g. easy/normal/hard) are a nice-to-have if it's
  cheap, but the config being centralized and readable is the actual
  requirement.

## Rendering

- Anaglyph stereo via three.js's `AnaglyphEffect`.
- HUD (score, lives) must render as a separate 2D overlay pass, NOT inside
  the anaglyph stereo render, or it will double-image and be unreadable.

## Scope

Single self-contained game. Prioritize a working, playable, good-looking
end-to-end loop over exhaustive feature coverage — if something in this
brief turns out to be a poor tradeoff for a one-shot build, use your
judgment, but note the deviation in your summary at the end.
