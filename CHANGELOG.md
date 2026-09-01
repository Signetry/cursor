# Changelog — signetry-cursor

Follows [Keep a Changelog](https://keepachangelog.com/) / [SemVer](https://semver.org/).

## [0.4.0] — 2026-09-01

> First release **tagged in this repository**. The `0.3.0` entry below describes a
> release cut in the pre-move repo, whose tag did not survive the org move.

### Changed

- **Licence: now [Apache-2.0](LICENSE)** (previously all rights reserved). Signetry
  moved to an open-core model: the integration surface — this repo included — is
  Apache-2.0, while the engine ([`Signetry/core`](https://github.com/Signetry/core))
  is BUSL-1.1 and converts to Apache-2.0 on 2030-08-31. See
  [LICENSING.md](https://github.com/Signetry/signetry/blob/main/LICENSING.md). The
  [CLA](CLA.md) still applies and was rewritten as a licence grant — contributors now
  keep their copyright.
- Naming: CLI `signetry`, import package `signetry_core`, env vars `SIGNETRY_*`, config
  `.signetry/`, and the Cursor rule file `signetry.mdc`.
- Install pin updated to
  `signetry-core @ git+https://github.com/Signetry/core@v0.8.0`; the advisory
  reviewer now installs `signetry-reviewer @ git+https://github.com/Signetry/reviewer@v0.3.0`.
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
