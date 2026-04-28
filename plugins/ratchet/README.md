# Ratchet

A productivity framework anchored in five Notion databases (Projects, Tasks, Action Log, Documentation, Clients), a shared vocabulary (Contexts and Primitives), and a small set of design principles (composable triangle, subtract first, no hidden memory).

This plugin packages the *methodology* — skills that read and write Ratchet records, framework docs, vocabulary, and a bootstrap flow for fresh adopters. **The plugin does not ship live database IDs or schemas.** Each consuming repo maintains its own `.ratchet/notion-config.json`; the plugin's skills read from that.

## What's in the box

```
plugins/ratchet/
├── skills/
│   ├── ratchet/             # Umbrella skill: create + query the 5 DBs
│   └── ratchet-bootstrap/   # One-shot scaffolding for a fresh Notion workspace
└── docs/
    ├── framework.md         # Contexts, Primitives, design principles
    ├── glossary.md          # Ratchet vocabulary
    ├── data-contract.md     # Shape and canonical location of consumer config
    ├── customization.md     # Supported deviations from the baseline schema
    └── openclaw-install.md  # How Forge / OpenClaw consumes this plugin
```

## Quick start

### Existing Ratchet workspace (Foundry-internal)

You already have the 5 DBs in Notion. Make sure your repo has `.ratchet/notion-config.json` populated per [`docs/data-contract.md`](./docs/data-contract.md), then call the `ratchet` skill from Claude.

### Fresh adopter (no Notion DBs yet)

Run the `ratchet-bootstrap` skill from Claude — it creates a "Ratchet" container page and the 5 DBs under it, then writes a starter `.ratchet/notion-config.json`.

## Requirements

- `NOTION_API_KEY` available as an env var (the skill reads it directly; consumers handle injection — Doppler, dotenv, etc.)
- A `.ratchet/notion-config.json` at the agent's working directory (or via the bootstrap skill)

## OpenClaw consumers

OpenClaw doesn't speak Claude Code marketplace. See [`docs/openclaw-install.md`](./docs/openclaw-install.md) for the filesystem-mount flow.
