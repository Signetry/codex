# Changelog — signetry-codex

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [Unreleased]

### Changed

- Naming: CLI `signetry`, env vars `SIGNETRY_*`, config `.signetry/`, import package
  `signetry_core`, and prose/docs references use **Signetry**. Install pins now use
  `signetry-core @ git+https://github.com/Signetry/core@v0.6.0` and
  `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.1.2`.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `signetry-plugins` monorepo into a dedicated repository under the
  [Signetry umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `signetry-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
