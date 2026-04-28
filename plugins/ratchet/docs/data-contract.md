# Data Contract

What consuming repos must provide for the Ratchet skills to work, and where it lives.

## Canonical path

**`.ratchet/notion-config.json`**, relative to the agent's working directory.

The skill reads this path directly — no env-var indirection, no per-consumer override. If your repo needs a different layout (because the file lives in `data/` or some other place for legacy reasons), symlink:

```bash
mkdir -p .ratchet
ln -s ../data/notion-config.json .ratchet/notion-config.json
```

## File shape

```json
{
  "_meta": {
    "last_updated": "2026-04-28T00:00:00Z",
    "ratchet_container": "<page-id of the 'Ratchet' container page in Notion, optional>",
    "bootstrap_version": "1"
  },
  "databases": {
    "clients": "<UUID>",
    "projects": "<UUID>",
    "tasks": "<UUID>",
    "action_log": "<UUID>",
    "documentation": "<UUID>"
  },
  "data_sources": {
    "clients": "<UUID>",
    "projects": "<UUID>",
    "tasks": "<UUID>",
    "action_logs": "<UUID>",
    "documentation": "<UUID>"
  },
  "properties": {
    "_optional": "Override property names if you've renamed any. Keys are role names (see customization.md); values are the actual property names in your Notion DB."
  },
  "views": {
    "_optional": "View IDs for any saved views you want skills to reference. Free-form."
  }
}
```

### Required keys

- `databases.{clients,projects,tasks,action_log,documentation}` — Notion *database* IDs (use these for `POST /v1/pages`).
- `data_sources.{clients,projects,tasks,action_logs,documentation}` — Notion *data source* IDs (use these for `POST /v1/data_sources/{id}/query`).

Note the slight inconsistency: `action_log` (singular) for the database key, `action_logs` (plural) for the data source key. This mirrors how Notion's Bridge has historically named them in `foundry-ops-bridge/data/notion-config.json`. Bootstrap emits both keys to match.

### Optional keys

- `_meta` — informational; not read by skills, but useful for tooling and humans.
- `properties` — only set when you've renamed canonical properties (see [customization.md](./customization.md)). Keys are *roles* (e.g., `tasks.title`, `tasks.due_date`); values are the actual property names in your Notion. Skills fall back to canonical names if a role is unset.
- `views` — free-form map of view IDs your other skills reference. Ratchet skills don't read this, but bridge/openclaw consumers may.

## Auxiliary data files (consumer-specific, not part of this contract)

The following are *operational* data files some consumers maintain. They are NOT part of the Ratchet plugin's data contract — bridge happens to have them, openclaw may or may not. Skills must not assume they exist.

| File | Purpose |
|---|---|
| `data/all-clients.json` | Bridge's client roster (Notion ID, Toggl prefix, billing rate) |
| `data/all-projects.json` | Bridge's project roster cross-referenced across Notion / Toggl / Replicon |

If you're writing a skill that wants project-name → page-URL resolution, query the Projects data source via the Notion API instead of reaching for `all-projects.json`. That keeps the skill portable across consumers.

## Setup paths

### New adopter

Run the `ratchet-bootstrap` skill. It creates the 5 DBs in Notion and emits `.ratchet/notion-config.json` for you.

### Existing Ratchet workspace (e.g., Foundry-internal)

Author `.ratchet/notion-config.json` by hand or copy from a peer repo. The values in foundry-ops-bridge's `data/notion-config.json` are the Foundry-canonical IDs.

### Migrating from a non-canonical path

```bash
# If your repo today has data/notion-config.json:
mkdir -p .ratchet
mv data/notion-config.json .ratchet/notion-config.json
# Or symlink if other tooling still references the old path:
ln -s ../data/notion-config.json .ratchet/notion-config.json
```

Update any consumer-side docs (CLAUDE.md, READMEs) that reference the old path.
