# phel-flappybird

A CLI **Flappy Bird** written in [Phel](https://phel-lang.org) (a Lisp that
compiles to PHP). Flap through scrolling pipes in your terminal.

```
+----------------------------------------------------------+
|                                          ###             |
|                                          ###             |
|                                                          |
|        @                                                 |
|                                          ###             |
|                                          ###             |
+----------------------------------------------------------+
Score: 3     High: 7     (space/up = flap, q = quit)
```

## Play

**From a release PHAR** (needs only PHP 8.4+):

```bash
# download phel-flappybird.phar from the Releases page, then:
php phel-flappybird.phar
```

**From source:**

```bash
composer install
composer dev          # run straight from source
# or compile an entry point and run it:
composer start        # = composer build && composer play
```

## Controls

| Key           | Action             |
| ------------- | ------------------ |
| `space` / `↑` | flap               |
| `q`           | quit               |
| `r`           | replay (game over) |

## Options

Pass `key=value` arguments to tune the game:

```bash
php phel-flappybird.phar width=80 height=30 gap=8 gravity=0.25
```

| Option      | Default | Meaning                      |
| ----------- | ------- | ---------------------------- |
| `width=N`   | 60      | board width                  |
| `height=N`  | 22      | board height                 |
| `gap=N`     | 6       | pipe gap height              |
| `spacing=N` | 22      | columns between pipes        |
| `gravity=F` | 0.20    | downward acceleration / tick |
| `flap=F`    | -0.95   | velocity applied on a flap   |
| `delay=N`   | 90000   | tick delay in microseconds   |
| `debug`     | off     | physics overlay              |

`--version` and `--help` print info and exit.

Your best score is saved to `~/.phel-flappybird-highscore`.

## Architecture

Functional core / imperative shell:

- `src/core/physics.phel` — **pure**. The whole game is one immutable world map
  (`{:bird :pipes :score :dead?}`) advanced by a single `step` reducer.
  Randomness is injected by the caller, so `step` is deterministic and fully
  unit-tested.
- `src/core/render.phel`, `src/core/input.phel` — the effectful edges (terminal
  drawing via [`phel-cli-gui`](https://github.com/Chemaclass/phel-cli-gui),
  keyboard reading).
- `src/game.phel` — the loop: read input → `step` → render → sleep → repeat.

## Development

```bash
composer test           # run the phel test suite
composer format         # format src + tests
composer build          # compile to out/
composer phar           # build build/out/phel-flappybird.phar
composer ci             # format-check + test + build
```

## Releasing

```bash
./tools/release.sh 0.1.0          # validate, build+smoke-test PHAR, tag, GH release
./tools/release.sh --dry-run      # preview without changing anything
```

## License

MIT
