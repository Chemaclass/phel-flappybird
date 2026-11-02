---
name: phel-conventions
description: Phel idioms and gotchas.
globs: "src/**/*.phel,tests/**/*.phel"
alwaysApply: true
---

- Namespaces use dots, one dependency per `(:require ...)` form: `(ns phel-flappybird.core.physics)`.
- `doseq` for side effects (render loops), `for` to build sequences.
- Integer `/` returns a `Ratio`, which `php/round` rejects. Divide by a float when the value feeds `php/round` or `int`: `(/ x 2.0)`.
- Vector order is not stable after `map`, `filter` or `push`. Compute order-independent values (rightmost pipe via `reduce max`), never `(last pipes)` for "newest".
- Read CLI args from `*argv*`, not `php/$argv`.
- Destructure binding-first: `{:keys [board]}` or `{gh :gap-height}`. Key-first `{:board board}` is deprecated.
- Use `assoc` and `conj`; `put` and `push` were removed in 0.53.
- Tests use `phel.test` (`deftest`, `is`), mirror the `src/` path with a `-test` suffix, and cover every pure function in `core/`.
- Terminal I/O goes through `chemaclass/phel-cli-gui` (`terminal-gui`): raw mode, non-blocking stdin, cursor, ANSI styles.
- Run `composer format` before committing; `composer ci` must pass. Find deprecations with `PHEL_WARN_DEPRECATIONS=1 composer test`.
