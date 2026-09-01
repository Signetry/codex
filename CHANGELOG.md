# Changelog — signetry-codex

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [0.4.0] — 2026-09-01

> First release **tagged in this repository**. The `0.3.0` entry below describes a
> release cut in the pre-move repo, whose tag did not survive the org move.

### Changed

- **Licence: Apache-2.0.** This repository moves from "All Rights Reserved" to
  [Apache-2.0](LICENSE) as part of Signetry's
  [open-core model](https://github.com/Signetry/signetry/blob/main/LICENSING.md) — the
  integration surface is Apache-2.0, while the engine
  ([`Signetry/core`](https://github.com/Signetry/core)) is BUSL-1.1 and converts to
  Apache-2.0 on 2030-08-31. The [CLA](CLA.md) is retained so contributions can move
  across the open-core line; it no longer withholds usage rights from contributors.

- Naming: CLI `signetry`, env vars `SIGNETRY_*`, config `.signetry/`, import package
  `signetry_core`, and prose/docs references use **Signetry**. Install pins now use
  `signetry-core @ git+https://github.com/Signetry/core@v0.8.0` and
  `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.3.0`.
- **The CLA's fallback licence grant is now non-exclusive.** It previously granted the
  Owner an *exclusive* licence where copyright assignment is not permitted by law, which
  would have stripped contributors of the right to use their own contribution — directly
  contradicting the rights the LICENSE grants everyone. The CLA text is now identical
  across all Signetry repositories (bar the engine/integration licence wording) so the
  legal terms cannot drift per-repo again. See [CLA.md](CLA.md) §2–3.

## [0.3.0] — 2026-07-26

### Added

- Split out of the `signetry-plugins` monorepo into a dedicated repository under the
  [Signetry umbrella](https://github.com/Signetry/signetry), per the platform
  architecture (one repo per integration).
- Pins `signetry-core>=0.3.0` (capability graph, plan binding, masked verifier,
  G1/G2/G3 gates, extension admission).
