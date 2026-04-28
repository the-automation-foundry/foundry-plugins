---
name: ratchet
description: Create and query Ratchet Notion records (Tasks, Action Log, Documentation, Projects, Clients). Reads database IDs from a consumer-local .ratchet/notion-config.json — does not hardcode UUIDs. Use when the user asks to log work, create a task, query upcoming tasks, file documentation, or otherwise interact with their Ratchet workspace.
metadata:
  openclaw:
    emoji: "🪜"
    requires:
      env: ["NOTION_API_KEY"]
    primaryEnv: "NOTION_API_KEY"
---

# Ratchet

Create and query records across the five Ratchet Notion databases: Projects, Tasks, Action Log, Documentation, Clients.

## Setup contract

Before any operation, this skill reads the consumer-local config at **`.ratchet/notion-config.json`** (relative to the current working directory). The file must contain `databases` and `data_sources` keys per [`docs/data-contract.md`](../../docs/data-contract.md). If the file is missing, run the `ratchet-bootstrap` skill or follow the data-contract doc to create one.

`NOTION_API_KEY` must be present in the environment. Do not echo or log it.

```bash
# Standard preamble for any operation:
test -f .ratchet/notion-config.json || { echo "Missing .ratchet/notion-config.json — see docs/data-contract.md"; exit 1; }
test -n "$NOTION_API_KEY" || { echo "NOTION_API_KEY not set"; exit 1; }
```

Use Notion API version `2025-09-03`. Use **data source IDs** for queries, **database IDs** for page creation. Prefer `curl` over the Notion MCP — the MCP `post-page` has a known double-serialization bug.

## Operations

Full curl templates with placeholders are in [`references/api-payloads.md`](./references/api-payloads.md). Load that file when you need exact payload shapes.

### Create Task

Required: `Task name`, `Project` (relation by page URL).
Optional: `Priority` (High/Medium/Low), `Effort Level` (Small/Medium/Large), `Due date`, `Status` (defaults to "Needs Definition" if unset), `Assignee`.

Resolve `Project` by name via the consumer's local cache (e.g., `data/all-projects.json` if present) or by querying the Projects data source.

### Create Action Log

Required: `Entry` (title), `Date` (YYYY-MM-DD), `Author` (`Nick` or `Forge`), `Type` (`Action` or `Decision`).
Optional: `Project` (relation), `Summary` (page body), `Billable hours`, `Non-billable hours`.

**Author convention:**
- **Nick** for entries the user logs themselves (slash-command flows, manual capture)
- **Forge** for entries the agent logs proactively (config changes, deployments, troubleshooting outcomes)

### Create Documentation

Required: `Request` (title), `Status`.
Optional: `Projects` (relation), `Platform`, `Tutorial URL`.

### Create Project

Required: `Project name`, `Client` (relation), `Status`.
Optional: `Parent project` (relation), `Priority`, `Area`.

Bridge users: don't call this directly — use the bridge's `/new-project` slash command, which wraps this and handles Toggl + taxonomy alongside Notion creation.

### Query Tasks

Filter by Project, Status, Priority, Due date, Author. Sort by Due date or Updated at. Use the Tasks data source ID from `notion-config.json`.

### Query Action Log

Filter by Project, Author, Date range, Type.

### Query Projects

Filter by Status, Client, Priority. Use for project-name → page-URL resolution when creating tasks/action-logs.

### Query Clients

List active clients. Use for client-name → page-URL resolution when creating projects.

## Proactive logging guidelines (Author=Forge)

When acting as Forge (OpenClaw / GCP VM context), automatically log to the Action Log on:

- Config changes (openclaw.json, Docker, workspace files)
- Deployments or rebuilds
- Security fixes
- Significant decisions or architecture changes
- Troubleshooting sessions that resolved an issue

Do **not** log:

- Routine heartbeat checks
- Read-only exploration or queries
- Trivial typo fixes

## Property roles (not names)

The skill depends on these *roles*, not exact property names. Consumers may rename properties in Notion as long as the role and type are preserved (see [`docs/customization.md`](../../docs/customization.md)):

| Database | Role | Required type |
|---|---|---|
| Tasks | title | `title` |
| Tasks | project relation | `relation` → Projects |
| Tasks | status | `status` |
| Action Log | title | `title` |
| Action Log | date | `date` |
| Action Log | author | `select` |
| Action Log | type | `select` |
| Documentation | title | `title` |
| Documentation | status | `status` (or `select`) |
| Projects | title | `title` |
| Projects | client relation | `relation` → Clients |
| Clients | title | `title` |

Read property *names* from the consumer's local `notion-config.json` `properties` map (optional; defaults to canonical names if absent).

## When NOT to use this skill

- The user wants a Toggl, Replicon, Reflect, or hosting operation — those are bridge-specific operational skills, not Ratchet.
- The user wants to interact with non-Ratchet Notion databases (Forge Planning, Foundry CEO Office content, etc.) — query those directly.
