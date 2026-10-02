# Maintenance notes — hermes-plugin-byterover

This repository is the `byterover` memory provider that shipped inside `NousResearch/hermes-agent` under
`plugins/memory/byterover/`. Nous Research moved every memory provider out of the core tree; this repo is
now its home, **maintained by Nous Research** and listed in the
[Hermes plugin catalog](https://hermes-agent.nousresearch.com/docs/plugins/byterover) as `byterover` (tier
`official`), pinned to a reviewed release tag. If ByteRover (https://byterover.dev) wants to own the plugin, open an issue here: we transfer the repo or re-point the catalog entry at their fork, keeping the `byterover` name so existing users migrate.

## Install (as a user)

```
hermes plugins install byterover     # from the catalog, at the reviewed pin
hermes memory setup            # or: set memory.provider: byterover in config.yaml
```

Users who already had `memory.provider: byterover` need do nothing: once core drops its bundled copy,
`hermes update` (and agent start) installs this plugin from the catalog automatically. Config
(`memory.byterover` / plugin sections) and data files are unchanged. The only external requirement is the `brv` CLI
(see README); nothing is installed into the Hermes venv.

## Keeping it in sync with core

Code here tracks the last in-tree copy (`git log -- plugins/memory/byterover` in hermes-agent) verbatim.
Every release is a tag `vX.Y.Z` matching `version` in `plugin.yaml`; the catalog
pin moves only through a hermes-agent PR that bumps `sha:` and `version:` together.

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
