# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and this project adheres
to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Flappy Bird in the terminal: gravity, flap, scrolling pipes, scoring.
- Pure functional core (`physics`) advanced by a single `step` reducer, with an
  imperative shell driving input, rendering, and timing.
- Persistent high score under `~/.phel-flappybird-highscore`.
- Configurable board, gap, spacing, gravity, flap velocity, and tick delay via
  `key=value` arguments; `--version` and `--help` flags.
- Single-file, self-contained PHAR build (`build/phar.sh`) and a guarded release
  script (`tools/release.sh`).

[Unreleased]: https://github.com/Chemaclass/phel-flappybird/commits/main
