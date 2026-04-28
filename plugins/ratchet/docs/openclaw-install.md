# OpenClaw Install

OpenClaw doesn't speak the Claude Code marketplace protocol — it has no `/plugin install` command. Skills come from local filesystem (the `.openclaw/skills/` directory) or via ClawHub (Notion's skill registry, not used here).

To consume the Ratchet plugin from an OpenClaw deployment, mount the plugin's skill directory into `.openclaw/skills/` via a filesystem clone + symlink.

## Why this works

OpenClaw's skill loader (`openclaw/src/agents/skills/local-loader.ts`) reads `SKILL.md` files with YAML frontmatter — the same shape Claude Code uses. The frontmatter the Ratchet skill ships includes `metadata.openclaw.*` fields (emoji, `requires.env`, `primaryEnv`) which OpenClaw uses for env validation and UI hints. Claude Code ignores those fields. One SKILL.md, two surfaces.

Reference: `openclaw/src/agents/skills/types.ts` documents the supported `metadata.openclaw.*` fields.

## Install on the GCP VM

```bash
# SSH to the VM:
gcloud compute ssh openclaw-gateway

# Clone the marketplace repo to a known location:
git clone git@github.com:the-automation-foundry/foundry-plugins.git ~/foundry-plugins

# Symlink the Ratchet skills into Forge's skill directory:
ln -s ~/foundry-plugins/plugins/ratchet/skills/ratchet \
      ~/sites/openclaw-gcp/.openclaw/skills/ratchet
ln -s ~/foundry-plugins/plugins/ratchet/skills/ratchet-bootstrap \
      ~/sites/openclaw-gcp/.openclaw/skills/ratchet-bootstrap
```

Adjust paths to wherever the OpenClaw deployment actually lives on your VM.

## Required env

The skill checks for `NOTION_API_KEY`. On the VM, this comes from Doppler (`foundry-openclaw/prd`) and is injected into the gateway's environment by `doppler run -- docker compose up`. No additional setup is needed — Doppler is already wired.

## Required local config

Create `.openclaw/.ratchet/notion-config.json` populated with the same DB IDs the bridge uses (these are the Foundry-canonical Ratchet workspace). Forge's working directory is `.openclaw/`, so the canonical relative path `.ratchet/notion-config.json` resolves to the right file.

```bash
mkdir -p ~/sites/openclaw-gcp/.openclaw/.ratchet
# Copy from a known-good source (e.g., scp from the bridge):
scp ~/sites/foundry-ops-bridge/data/notion-config.json \
    openclaw-gateway:~/sites/openclaw-gcp/.openclaw/.ratchet/notion-config.json
```

## Update flow

```bash
ssh openclaw-gateway
cd ~/foundry-plugins && git pull
# The symlinks pick up the new content automatically.
# Restart the gateway so Forge re-reads SKILL.md:
docker compose -f ~/sites/openclaw-gcp/.openclaw/docker-compose.yml restart
```

You can put `cd ~/foundry-plugins && git pull` in a daily cron if you want auto-updates. The skill body changes take effect on next gateway restart.

## Replacing the old `notion-action-log` skill

If your deployment still has `.openclaw/skills/notion-action-log/` from before the plugin existed:

```bash
rm -rf ~/sites/openclaw-gcp/.openclaw/skills/notion-action-log
```

The old skill referenced the deprecated Forge Planning Action Log DB (`b51997e3-…`). The Ratchet plugin uses the new five-DB Action Log; functionality is fully covered.

Update `.openclaw/workspace/TOOLS.md` to point at the `ratchet` skill instead of `notion-action-log`.
