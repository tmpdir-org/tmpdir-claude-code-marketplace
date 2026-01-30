# Changelog

## [Unreleased]

## [1.1.1] - 2026-01-30

- update review to not tag files

## [1.0.9] - 2026-01-20

### Added

- `/review` command for multi-agent code review.
- Plan tracking via `plans/plans.md` with one active plan at a time.
- **Paused** status for plans interrupted by starting a new plan.
- Starting commit hash recorded when creating plans for easier diff review.
- Automatic commit tracking in plan files during `/implement` and
  `/implement-tests`.

### Changed

- `/implement` and `/implement-tests` now create granular commits per logical
  unit.
- `/review` appends results directly to the active plan file.

## [1.0.8] - 2025-12-05

### Improved

- Release command now adds a new `[Unreleased]` section after creating a
  release.

## [1.0.7] - 2025-12-05

### Added

- `/release` command to automate version releases with changelog updates and git
  tagging.

## [1.0.6] - 2025-12-05

### Improved

- Documentation style guidelines now enforce consistent punctuation in lists.

## [1.0.5] - 2025-12-05

### Added

- `/execute` command for surgical code changes using inline instruction markers.

## Previous releases

See git log for earlier changes.
