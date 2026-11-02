---
name: architecture
description: Layout, functional-core architecture, commands and release.
globs: "**/*"
alwaysApply: true
---

Terminal Flappy Bird in [Phel](https://phel-lang.org). Requires PHP >= 8.5 and phel-lang ^0.53.

```
src/main.phel          CLI entry: --version / --help / run the game
src/config.phel        parse key=value args into a config map
src/game.phel          loop (imperative shell): input -> step -> render -> sleep
src/util.phel          arg helpers
src/highscore.phel     best score in ~/.phel-flappybird-highscore
src/core/physics.phel  PURE core: world value and `step` reducer (no I/O)
src/core/render.phel   terminal drawing via chemaclass/phel-cli-gui
src/core/input.phel    non-blocking stdin
src/core/version.phel  the only version literal
tests/                 phel.test files mirroring src (`-test` suffix)
```

Functional core, imperative shell:

- The game is one immutable world map `{:bird :pipes :score :dead?}` advanced by the pure `step` reducer in `core/physics.phel`. No I/O and no randomness inside `step`: the caller passes the next pipe gap in, so `step` stays deterministic and testable.
- `game.phel` is the only stateful part: read input, call `step`, render the diff, sleep, recur.
- `render.phel` and `input.phel` are the effectful edges. Put game logic in `physics.phel` with a test, never there.

Commands: `composer dev`, `test`, `lint`, `format`, `build` (to `out/`), `phar` (`build/out/phel-flappybird.phar`), `ci` (validate, format-check, lint, test, build).

Release: `./tools/release.sh [X.Y.Z]` validates, builds and smoke-tests the PHAR, tags, pushes and creates the GitHub release. It bumps the version literal in `src/core/version.phel`.
