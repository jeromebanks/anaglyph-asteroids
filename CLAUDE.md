# CLAUDE.md

Project conventions for working in this repo. This is a from-scratch
one-shot build (see INIT.md for the game design brief) — treat this file
as the standing rules for *how* to build it, not *what* to build.

## Project

3D anaglyph Asteroids clone in three.js. Single-page browser game, no
backend. Optimize for: it runs by opening `index.html` (or with a trivial
`npm run dev` if you pull in tooling that needs one — avoid that unless
there's a real reason).

## Working style

- Prefer explicit, readable code over clever code. Favor small, named
  functions over long inline logic, especially in the game loop.
- Centralize tunable constants (difficulty, spawn rates, speeds, sizes)
  in one place — see INIT.md's Difficulty section. Don't scatter magic
  numbers through gameplay logic.
- Decouple systems where it's cheap to: rendering, input, physics/motion,
  and game state (score, lives, wave number) should be reasoning-about-able
  independently, even in a single-file or few-file build. Doesn't need to
  be over-architected — this is a game jam-scale project, not a platform.
- If you make a nontrivial design call not specified in INIT.md (e.g. how
  saucer aim-inaccuracy decays, exact wave-scaling curve, control scheme
  details), note it briefly in your final summary rather than silently
  deciding and moving on. I'd rather see 5 bullet points of "here's what
  I decided and why" than nothing.
- No physics engine dependency unless you judge it genuinely simplifies
  the build — hand-rolled vector math is expected to be sufficient and
  is fine.

## File structure

Use your judgment on single-file vs. split-file — this is small enough
that either is reasonable. If you split, prefer clear separation like:

```
index.html
src/
  main.js          # boot, render loop
  scene.js         # camera, lighting, anaglyph setup
  ship.js
  asteroids.js
  saucers.js
  input.js
  difficulty.js    # tunable config, see INIT.md
  hud.js           # separate 2D overlay, not part of stereo render
```

Don't pull in a bundler/framework for this — plain ES modules or a single
file is preferable to adding build tooling for a project this size.

## Testing / verification

- No formal test suite expected for a game this size. Instead: after
  building, describe how you manually verified core loops work (ship
  moves/wraps/shoots, asteroids split, saucers spawn and are dangerous,
  anaglyph renders, HUD doesn't double-image).
- If you can run a headless check (e.g. the page loads without console
  errors) do that as a sanity check before declaring done.

## Scope discipline

This should stay a single focused build. If you find yourself wanting to
add features not in INIT.md (power-ups, multiplayer, menus beyond
start/game-over, etc.), don't — flag them as ideas in your summary instead
and stop there.
