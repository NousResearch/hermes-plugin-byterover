> **Unmaintained — looking for an owner.** Hermes Agent removes its bundled `byterover` memory provider from core on **October 15, 2026**. This repo is a standalone copy of that provider, kept by Nous Research only so someone can take it over: Nous does not maintain it, publish fixes for it, or list it in the Hermes plugin catalog. We've asked ByteRover to take it over (https://github.com/campfirein/brv-openclaw-plugin/issues/33). ByteRover or anyone else who wants to own it: open an issue here and we'll help with the handoff (repo transfer or fork, plus the catalog entry that lets `hermes update` move existing users onto your plugin). See [HANDOFF.md](HANDOFF.md).

# ByteRover Memory Provider

Persistent memory via the `brv` CLI — hierarchical knowledge tree with tiered retrieval (fuzzy text → LLM-driven search).

## Requirements

Install the ByteRover CLI:
```bash
curl -fsSL https://byterover.dev/install.sh | sh
# or
npm install -g byterover-cli
```

## Setup

```bash
hermes memory setup    # select "byterover"
```

Or manually:
```bash
hermes config set memory.provider byterover
# Optional cloud sync:
echo "BRV_API_KEY=your-key" >> ~/.hermes/.env
```

## Config

| Env Var | Required | Description |
|---------|----------|-------------|
| `BRV_API_KEY` | No | Cloud sync key (optional, local-first by default) |

Working directory: `$HERMES_HOME/byterover/` (profile-scoped).

## Tools

| Tool | Description |
|------|-------------|
| `brv_query` | Search the knowledge tree |
| `brv_curate` | Store facts, decisions, patterns |
| `brv_status` | CLI version, tree stats, sync state |
