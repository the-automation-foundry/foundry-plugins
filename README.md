# Foundry Plugins

The Automation Foundry's Claude Code plugin marketplace.

## Plugins

| Plugin | Description |
|---|---|
| [`ratchet`](./plugins/ratchet) | Ratchet productivity framework — Notion-backed methodology, skills, and vocabulary |

## Install (Claude Code)

```
/plugin marketplace add the-automation-foundry/foundry-plugins
/plugin install ratchet@foundry-plugins
```

To update: `/plugin update ratchet`. The plugin manifest omits `version`, so updates track the latest commit on the marketplace's default branch.

## Install (OpenClaw)

OpenClaw doesn't speak the Claude Code marketplace protocol. See [`plugins/ratchet/docs/openclaw-install.md`](./plugins/ratchet/docs/openclaw-install.md) for the host-side bootstrap-script pattern (clones a pinned tag, symlinks skills into the workspace).

## Releases

Tagged releases for consumers (like the OpenClaw bootstrap script) that need a deterministic ref:

- [`v0.1.0`](https://github.com/the-automation-foundry/foundry-plugins/releases/tag/v0.1.0) — initial scaffold

## Adding a new plugin

1. Create `plugins/<name>/` with the standard layout (`.claude-plugin/plugin.json`, `skills/`, etc.).
2. Add an entry to `.claude-plugin/marketplace.json`.
3. PR.
