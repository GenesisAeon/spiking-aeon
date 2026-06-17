# Contributing

Thanks for your interest in contributing to `spiking-aeon`, the GenesisAeon
Neuromorphic SNN Hardware Bridge (Package 26)!

## Getting started

1. Fork and clone the repository.
2. Create a virtual environment: `python -m venv .venv && source .venv/bin/activate`
   (or `.venv\Scripts\activate` on Windows).
3. Install in editable mode with dev dependencies: `pip install -e ".[dev]"`.
4. Run the test suite: `pytest`.

## Code style

- Format and lint with `ruff` (`ruff check src tests`).
- Type-check with `mypy src` (strict mode is enabled).
- Keep functions documented with concise docstrings.

## Diamond Interface

`spiking_aeon.system.SpikingAeon` implements the GenesisAeon Diamond
Interface (`run_cycle`, `get_crep_state`, `get_utac_state`,
`get_phase_events`, `to_zenodo_record`). Any change to these methods'
signatures or return shapes is a **breaking change** and requires a MAJOR
version bump (see `RELEASE_GUIDE.md`).

## Pull requests

- One logical change per PR.
- Add or update tests for any behavioral change.
- Update `CHANGELOG.md` under an `## [Unreleased]` section.
- Fill out the PR template (`.github/PULL_REQUEST_TEMPLATE.md`).

## Reporting issues

Please use the issue templates in `.github/ISSUE_TEMPLATE/` — they help us
triage bug reports vs. feature requests quickly.

## Scientific claims

This package is part of a research framework. If your contribution
touches the SNN UTAC model, CREP→LIF mapping, or the NeuEdge/Loihi 2
benchmark figures, please cite the source (paper, dataset, or prior
GenesisAeon Zenodo record) and clearly mark speculative vs. validated
claims.
