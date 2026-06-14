---
name: phel-conventions
description: Phel idioms and gotchas for this codebase.
globs: "src/**/*.phel,tests/**/*.phel"
alwaysApply: true
---

# Phel conventions

- **Namespaces use dots**, not backslashes: `(ns phel-flappybird.core.physics)`,
  one dependency per `(:require ...)` form.
- **`doseq` for side effects, `for` for building sequences.** Rendering loops
  that draw/erase are side effects — use `doseq`.
- **Integer `/` yields a `Ratio`**, which `php/round` rejects. When a value
  feeds `php/round`/`int`, divide by a float (`(/ x 2.0)`).
- **Do not rely on vector order** after `map`/`filter`/`push`. Compute
  order-independent values (e.g. rightmost pipe via `reduce max`), never
  `(last pipes)` to mean "newest".
- Use `argv` for CLI args, not `php/$argv`.
- Tests use `phel.test` (`deftest`/`is`) and mirror the `src/` path with a
  `-test` suffix. Every pure function in `core/` gets a test.
- Run `composer format` before committing; `composer ci` must pass.
- Terminal I/O goes through `chemaclass/phel-cli-gui` (`terminal-gui`), which
  handles raw mode, non-blocking stdin, cursor, and ANSI styles.
