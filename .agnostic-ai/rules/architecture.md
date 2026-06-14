---
name: architecture
description: phel-flappybird project layout, architecture, and commands.
globs: "**/*"
alwaysApply: true
---

# phel-flappybird

Flappy Bird for the terminal, written in [Phel](https://phel-lang.org) (a Lisp
that compiles to PHP). Requires PHP >= 8.4 and phel-lang ^0.44.

## Layout

```
src/main.phel          CLI entry: --version / --help / run the game
src/config.phel        parse key=value args into a config map
src/game.phel          the loop (imperative shell): input -> step -> render -> sleep
src/util.phel          arg helpers
src/highscore.phel     persist best score to ~/.phel-flappybird-highscore
src/core/physics.phel  PURE core: world value + `step` reducer (no I/O)
src/core/render.phel   terminal drawing via chemaclass/phel-cli-gui
src/core/input.phel    keyboard reading (non-blocking stdin)
src/core/version.phel  single source of truth for the version string
tests/                 phel.test deftest files, mirroring src layout (`-test` suffix)
```

## Architecture: functional core / imperative shell

- The whole game is ONE immutable world map `{:bird :pipes :score :dead?}`,
  advanced by the pure `step` reducer in `core/physics.phel`. Keep it pure:
  no I/O, no randomness inside `step` — randomness (the next pipe gap) is
  **injected** by the caller as an argument, so `step` stays deterministic and
  unit-testable.
- `game.phel` is the only stateful part: read input, call `step`, render the
  diff, sleep, recur.
- `render.phel` / `input.phel` are the effectful edges. Never put game logic
  there; put it in `physics.phel` with a matching test.

## Commands

```
composer dev          run from source (vendor/bin/phel run phel-flappybird.main)
composer test         run the phel test suite
composer format       format src + tests
composer build        compile to out/
composer phar         build the self-contained build/out/phel-flappybird.phar
composer ci           format-check + test + build
```

Release with `./tools/release.sh [X.Y.Z]` (validates, builds + smoke-tests the
PHAR, tags, pushes, creates the GitHub release). The version literal lives in
`src/core/version.phel`; `release.sh` bumps it.
