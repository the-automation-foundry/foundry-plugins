# OpenClaw Install

OpenClaw doesn't speak the Claude Code marketplace protocol — it has no `/plugin install` command. Skills come from the local filesystem (`.openclaw/skills/`) or from ClawHub. Distribution-wise, we treat OpenClaw as a regular consumer that reads `SKILL.md` files from a directory; we just have to make sure those files are present on the host before `docker compose up`.

The OpenClaw Docker setup typically mounts the host's `.openclaw/` directory as a volume, which means anything baked into the image at `/home/node/.openclaw/skills/` is masked at runtime. Skills must live on the host. The recommended pattern is a small idempotent bootstrap script committed to the workspace repo that clones this plugin at a pinned tag and symlinks each skill into `.openclaw/skills/`.

## Why this works

OpenClaw's skill loader (`openclaw/src/agents/skills/local-loader.ts`) reads `SKILL.md` files with YAML frontmatter — the same shape Claude Code uses. The frontmatter the Ratchet skill ships includes `metadata.openclaw.*` fields (emoji, `requires.env`, `primaryEnv`) which OpenClaw uses for env validation and UI hints; Claude Code ignores those. One SKILL.md, two surfaces.

Reference: `openclaw/src/agents/skills/types.ts` documents the supported `metadata.openclaw.*` fields.

## Recommended pattern: bootstrap script

Add `.openclaw/scripts/install-plugins.sh` to your workspace repo:

```bash
#!/usr/bin/env bash
set -euo pipefail

PLUGIN_TAG="${FOUNDRY_PLUGINS_TAG:-v0.1.0}"
WORKSPACE_DIR="$(cd "$(dirname "$0")/.." && pwd)"
PLUGIN_DIR="${WORKSPACE_DIR}/.foundry-plugins"
SKILLS_DIR="${WORKSPACE_DIR}/skills"

mkdir -p "$SKILLS_DIR"

if [ -d "$PLUGIN_DIR/.git" ]; then
  git -C "$PLUGIN_DIR" fetch --tags --depth 1 origin
  git -C "$PLUGIN_DIR" checkout --quiet "$PLUGIN_TAG"
else
  git clone --depth 1 --branch "$PLUGIN_TAG" \
    https://github.com/the-automation-foundry/foundry-plugins.git "$PLUGIN_DIR"
fi

for skill in "$PLUGIN_DIR"/plugins/ratchet/skills/*/; do
  skill_name="$(basename "$skill")"
  ln -sfn "$skill" "$SKILLS_DIR/$skill_name"
done

echo "Installed plugin skills at tag $PLUGIN_TAG: $(ls "$PLUGIN_DIR/plugins/ratchet/skills/" | tr '\n' ' ')"
```

Add to `.openclaw/.gitignore`:

```
.foundry-plugins/
```

Wire it into your spin-up flow — either document `./scripts/install-plugins.sh && doppler run -- docker compose up` as the deploy command, or add a `Makefile`:

```make
up:
	./scripts/install-plugins.sh
	doppler run -- docker compose up -d --build
```

## Bumping the plugin version

Edit `PLUGIN_TAG` in `install-plugins.sh` (or override at runtime with `FOUNDRY_PLUGINS_TAG=v0.2.0 ./scripts/install-plugins.sh`), commit, redeploy. The pinned tag lives in workspace git history.

## Required env

The skill checks for `NOTION_API_KEY`. Doppler injects it into the container via the docker-compose `environment` block. No additional setup needed — Doppler is already wired in `foundry-openclaw/prd`.

## Required local config

Create `.openclaw/.ratchet/notion-config.json` populated with the same DB IDs the bridge uses (these are the Foundry-canonical Ratchet workspace). When Forge runs, its working directory is the mounted `.openclaw/` (or wherever the gateway sets `cwd`), so the canonical relative path `.ratchet/notion-config.json` resolves to the right file.

```bash
mkdir -p ~/sites/openclaw-gcp/.openclaw/.ratchet
# Copy from a known-good source (e.g., scp from your bridge clone, or copy values
# from foundry-ops-bridge/.ratchet/notion-config.json once that migration completes):
cp ~/sites/foundry-ops-bridge/data/notion-config.json \
   ~/sites/openclaw-gcp/.openclaw/.ratchet/notion-config.json
```

## Replacing the old `notion-action-log` skill

If your deployment still has `.openclaw/skills/notion-action-log/` from before the plugin existed:

```bash
rm -rf ~/sites/openclaw-gcp/.openclaw/skills/notion-action-log
```

The old skill referenced the deprecated Forge Planning Action Log DB (`b51997e3-…`). The Ratchet plugin uses the new five-DB Action Log; functionality is fully covered.

Update `.openclaw/workspace/TOOLS.md` to point at the `ratchet` skill instead of `notion-action-log`.

## Alternative: bake into the image (advanced)

If you want skills bundled in the Docker image rather than the host, refactor the docker-compose volume mounts to be more granular — keep `workspace/` and `openclaw.json` mounted from the host, but exclude `skills/` from the mount so the image's `/home/node/.openclaw/skills/` survives. Then add a `RUN git clone --depth 1 --branch <tag> https://github.com/the-automation-foundry/foundry-plugins.git /opt/foundry-plugins && ln -s ...` to the Dockerfile.

This is more invasive — it changes the deployment shape — and most setups don't need it. The bootstrap-script pattern above is the recommended path.
