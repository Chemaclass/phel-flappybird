# Changelog

All notable changes to this project are documented here. The format is based on
[Keep a Changelog](https://keepachangelog.com/en/1.0.0/) and this project adheres
to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Changed
- Requires PHP 8.5+ and Phel 0.53 (phel-cli-gui 0.16).
- `composer dev` / `composer play` no longer stop after five minutes (Composer process timeout disabled).
- `composer ci` also validates `composer.json` and lints; CI installs from the lock file, runs on pull requests, and smoke-tests the PHAR.

## [0.1.0] - 2026-06-14

### Added
- Flappy Bird in the terminal: flap through scrolling pipes, score, high score.
- Configurable board and physics via `key=value` args; `--version` / `--help`.
- Self-contained PHAR build and release script.
