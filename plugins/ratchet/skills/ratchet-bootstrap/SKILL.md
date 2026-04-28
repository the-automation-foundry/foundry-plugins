---
name: ratchet-bootstrap
description: One-shot scaffolding for a fresh Ratchet adopter. Creates a Ratchet container page + the 5 baseline Notion databases (Projects, Tasks, Action Log, Documentation, Clients) under a user-supplied parent, then writes a starter .ratchet/notion-config.json. Use only when the user does NOT already have a Ratchet workspace — existing adopters should not run this.
metadata:
  openclaw:
    emoji: "🌱"
    requires:
      env: ["NOTION_API_KEY"]
    primaryEnv: "NOTION_API_KEY"
---

# Ratchet Bootstrap

Set up a fresh Ratchet workspace in Notion and emit a starter local config. Run once per repo that needs to consume the Ratchet plugin.

## When NOT to run this

- You already have Ratchet databases in Notion. Author `.ratchet/notion-config.json` by hand instead — see [`docs/data-contract.md`](../../docs/data-contract.md).
- You're on a Foundry-internal repo (foundry-ops-bridge, openclaw-gcp). Those use the existing Foundry Ratchet workspace; copy `notion-config.json` from a peer repo, don't run this.

## Prerequisites

- `NOTION_API_KEY` in env, with a Notion integration that has been granted access to the parent page where the Ratchet workspace will live.
- A Notion *parent page* picked out — the script creates a "Ratchet" container page under it, then the 5 DBs as children of the container.
- `jq` and `curl` available locally.

## Flow

1. **Confirm prerequisites** — check `NOTION_API_KEY` is set and `jq` is installed. Stop with an actionable error if not.

2. **Ask for the parent page.** Prompt the user for one of:
   - A Notion page URL (e.g., `https://www.notion.so/Workspace-abc123…`)
   - A page ID (UUID, with or without dashes)

   Extract the UUID and validate by calling `GET /v1/pages/{id}` — confirm 200 OK.

3. **Create the "Ratchet" container page** as a child of the parent:

   ```bash
   curl -s -X POST "https://api.notion.com/v1/pages" \
     -H "Authorization: Bearer $NOTION_API_KEY" \
     -H "Notion-Version: 2025-09-03" \
     -H "Content-Type: application/json" \
     -d '{
       "parent": {"page_id": "${PARENT_ID}"},
       "properties": {
         "title": [{"text": {"content": "Ratchet"}}]
       },
       "icon": {"emoji": "🪜"},
       "children": [
         {
           "object": "block", "type": "paragraph",
           "paragraph": {"rich_text": [{"text": {"content": "Ratchet productivity framework. Five databases below; see foundry-plugins for skills and docs."}}]}
         }
       ]
     }'
   ```

   Capture the new page's `id` as `RATCHET_CONTAINER_ID`.

4. **Create the 5 databases** as children of the Ratchet container, in this order: Clients, Projects, Tasks, Action Log, Documentation (Projects depends on Clients; Tasks depends on Projects; Action Log depends on Projects).

   Use the baseline schemas in §Baseline schemas below. After each `POST /v1/databases`, capture the returned `id` (the database ID) and the `data_sources[0].id` (the data source ID — needed for queries).

5. **Emit `.ratchet/notion-config.json`** to the repo root (or wherever the user confirms). Shape:

   ```json
   {
     "_meta": {
       "created": "ISO-8601 UTC timestamp",
       "ratchet_container": "<RATCHET_CONTAINER_ID>",
       "bootstrap_version": "1"
     },
     "databases": {
       "clients": "<CLIENTS_DB_ID>",
       "projects": "<PROJECTS_DB_ID>",
       "tasks": "<TASKS_DB_ID>",
       "action_log": "<ACTION_LOG_DB_ID>",
       "documentation": "<DOCS_DB_ID>"
     },
     "data_sources": {
       "clients": "<CLIENTS_DS_ID>",
       "projects": "<PROJECTS_DS_ID>",
       "tasks": "<TASKS_DS_ID>",
       "action_logs": "<ACTION_LOG_DS_ID>",
       "documentation": "<DOCS_DS_ID>"
     }
   }
   ```

6. **Print next steps.** Tell the user:
   - The container page URL, so they can go open it in Notion
   - The path to the new `.ratchet/notion-config.json`
   - To add `.ratchet/` to `.gitignore` if the IDs are sensitive (they typically are for private workspaces)
   - To run the `ratchet` skill to start creating tasks/action-logs

## Baseline schemas

These are the *recommended* baseline shapes. Renames and additions are supported post-bootstrap — see [`docs/customization.md`](../../docs/customization.md).

### Clients
- `Client name` (title)
- `Status` (status: Active / Inactive / Archived)

### Projects
- `Project name` (title)
- `Client` (relation → Clients)
- `Parent project` (relation → Projects, self-referential, optional)
- `Status` (status: Not Started / In Progress / On Hold / Done / Archived)
- `Priority` (select: High / Medium / Low)

### Tasks
- `Task name` (title)
- `Project` (relation → Projects)
- `Status` (status: Needs Definition / Blocked / Ready / In Progress / Done / Canceled)
- `Priority` (select: High / Medium / Low)
- `Effort Level` (select: Small / Medium / Large)
- `Due date` (date)
- `Assignee` (people)

### Action Log
- `Entry` (title)
- `Date` (date)
- `Author` (select: Nick / Forge — extend per consumer)
- `Type` (select: Action / Decision)
- `Project` (relation → Projects)
- `Billable hours` (number)
- `Non-billable hours` (number)

### Documentation
- `Request` (title)
- `Status` (status: Draft / Published / Archived)
- `Projects` (relation → Projects)
- `Platform` (select: free-form per consumer)
- `Tutorial URL` (url)

## Implementation notes

- Notion API version: `2025-09-03`.
- Use `POST /v1/databases` for each DB; the response includes a `data_sources` array — pin to `[0]` for single-source DBs.
- For relation properties to other DBs, set `database_id` to the target DB's ID. Self-referential relations (Tasks→Tasks for "Blocked by") use the same DB's own ID.
- Status property type uses the `status` shape (not `select`). The Notion API auto-creates status groups; you can specify `groups` and `options` in the property definition.
- After all 5 DBs are created, do a final smoke test: query each data source with `page_size: 1` and confirm 200 OK.
