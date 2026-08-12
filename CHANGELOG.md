# Changelog — umbra-codex

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [Unreleased]

### Changed

- Rebranded the platform from Umbra to Signetry: CLI `umbra` → `signetry`, env
  `UMBRA_*` → `SIGNETRY_*`, config `.umbra/` → `.signetry/`, imports `umbra_core`
  → `signetry_core`, and prose/docs references. Install pins now use
  `signetry-core @ git+https://github.com/Signetry/core@v0.6.0` and
  `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.1.2`.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `umbra-plugins` monorepo into a dedicated repository under the
  [Umbra umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `umbra-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
