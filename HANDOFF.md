# Maintenance notes — hermes-plugin-byterover

**Status: unmaintained, looking for an owner.** The bundled copy leaves Hermes core on **October 15, 2026**. Nous Research does not maintain memory providers. This
repository is a standalone copy of the `byterover` memory provider that ships inside `NousResearch/hermes-agent`
under `plugins/memory/byterover/`, prepared so someone else can take it over. It is **not** listed in the
Hermes plugin catalog and Nous publishes no further fixes or releases here. The last sync with core is
tag `v1.0.2`.

## Taking it over

ByteRover (https://byterover.dev) — or anyone else — who wants to maintain this provider: open an issue in this repo. We can
transfer the repository to you, or you can fork it. Once you maintain it, submit a catalog entry to
`NousResearch/hermes-agent` (`plugin-catalog/byterover.yaml`, see the
[plugin catalog guide](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugin-catalog)) under the
name `byterover`. That exact name matters: when core later drops its bundled copy, `hermes update` installs the
catalog plugin of the same name for users who have `memory.provider: byterover`, keeping their config and data.

## Install (as a user)

Until October 15, 2026 Hermes Agent still bundles this provider (`hermes memory setup`, or
`memory.provider: byterover` in config.yaml). After that, unless a new owner has listed it in the catalog, install this
unmaintained copy by hand: `hermes plugins install NousResearch/hermes-plugin-byterover --ref a3876250252afb098ffc038d6194b0ebfb364b9c` (tag `v1.0.2`).
Same provider name, config and data, so nothing else changes.
From a non-catalog source the install scanner blocks it with a `caution` verdict (the `curl … | sh`
`brv` install hint and the `brv` subprocess calls); review the findings, then add `--force`.

## Keeping it in sync with core

Code here tracks the last in-tree copy (`git log -- plugins/memory/byterover` in hermes-agent) verbatim.
Every release is a tag `vX.Y.Z` matching `version` in `plugin.yaml`. A future catalog entry would pin a tag by `sha:` and `version:`.

Differences versus the in-tree copy (mechanical only; behaviour is identical):

- Self-imports are relative so the package loads from `~/.hermes/plugins/byterover/` under the loader's
  synthetic namespace.
- No `pyproject.toml`: the provider has no Python dependencies, and an empty `pyproject.toml` still
  makes Hermes ask for dependency consent at install, which an unattended migration (agent start,
  gateway) cannot answer. Add one only together with a real dependency.

## For maintainers

- `tests/` holds the in-tree tests (`tests/plugins/memory/test_byterover*.py` in hermes-agent) with the
  import path switched to the package `tests/conftest.py` loads from this repo. Run them locally with
  `HERMES_AGENT_REPO=~/.hermes/hermes-agent PYTHONPATH=~/.hermes/hermes-agent python -m pytest -q`;
  CI runs the same on Linux, macOS and Windows against hermes-agent `main`.
- While a Hermes install still carries the bundled `plugins/memory/byterover`, that copy wins on name and
  this plugin is dormant; it takes over once core drops the bundled directory.

Original authors are preserved in hermes-agent's history: `git log -- plugins/memory/byterover`.
