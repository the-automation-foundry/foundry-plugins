# Foundry Plugins

The Automation Foundry's Claude Code plugin marketplace.

## Plugins

| Plugin | Description |
|---|---|
| [`ratchet`](./plugins/ratchet) | Ratchet productivity framework — Notion-backed methodology, skills, and vocabulary |

## Install

While this repo is private, `/plugin marketplace add the-automation-foundry/foundry-plugins` may not authenticate cleanly (see `anthropics/claude-code` issues #9756, #17201). The reliable path is to clone manually and add as a local marketplace:

```bash
git clone git@github.com:the-automation-foundry/foundry-plugins.git \
  ~/.claude/plugins/marketplaces/the-automation-foundry/foundry-plugins
```

Then in Claude Code:

```
/plugin marketplace add ~/.claude/plugins/marketplaces/the-automation-foundry/foundry-plugins
/plugin install ratchet@foundry-plugins
```

To update later:

```bash
cd ~/.claude/plugins/marketplaces/the-automation-foundry/foundry-plugins && git pull
```

then `/plugin update ratchet` in any Claude session.

When the upstream private-repo auth issue resolves, the simpler form will work:

```
/plugin marketplace add the-automation-foundry/foundry-plugins
```

## Adding a new plugin

1. Create `plugins/<name>/` with the standard layout (`.claude-plugin/plugin.json`, `skills/`, etc.).
2. Add an entry to `.claude-plugin/marketplace.json`.
3. PR.
