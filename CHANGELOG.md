# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]
### Changed
- Relicensed from MIT to a dual license: code under
  **GPL-3.0-or-later** (`LICENSE`), documentation under
  **CC BY 4.0** (`LICENSE-DOCS`).

## [1.0.0] - 2026
### Added
- Initial v1.0.0 release as part of the GenesisAeon ecosystem-wide 1.0.0
  milestone.
- Diamond Interface (`run_cycle`, `get_crep_state`, `get_utac_state`,
  `get_phase_events`, `to_zenodo_record`) on `spiking_aeon.system.SpikingAeon`.
- Standardized release tooling: `.zenodo.json`, GitHub Actions release
  workflow (`.github/workflows/release.yml`), `RELEASE_GUIDE.md`,
  `CONTRIBUTING.md`, issue/PR templates.

### Changed
- Project metadata (`pyproject.toml`) version bumped to `1.0.0` to match
  the ecosystem-wide release milestone.
