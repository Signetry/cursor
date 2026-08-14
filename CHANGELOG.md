# Changelog — signetry-cursor

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [Unreleased]

### Changed

- Naming: CLI `signetry`, import package `signetry_core`, env vars `SIGNETRY_*`, config
  `.signetry/`, and the Cursor rule file `signetry.mdc`.
- Install pin updated to
  `signetry-core @ git+https://github.com/Signetry/core@v0.6.0`; the advisory
  reviewer now installs `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.1.2`.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `signetry-plugins` monorepo into a dedicated repository under the
  [Signetry umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `signetry-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
