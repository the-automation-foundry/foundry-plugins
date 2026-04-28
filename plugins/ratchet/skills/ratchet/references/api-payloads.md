# Ratchet API Payloads

Curl templates for create + query operations against the five Ratchet databases. Load this file when you need exact payload shapes.

All examples assume:
- `NOTION_API_KEY` is in the environment
- `.ratchet/notion-config.json` is present in the working directory

```bash
NOTION_VERSION="2025-09-03"
NOTION_AUTH="Authorization: Bearer ${NOTION_API_KEY}"
NOTION_VER="Notion-Version: ${NOTION_VERSION}"

# Pull DB / data source IDs from local config:
TASKS_DB=$(jq -r '.databases.tasks' .ratchet/notion-config.json)
TASKS_DS=$(jq -r '.data_sources.tasks' .ratchet/notion-config.json)
ACTION_LOG_DB=$(jq -r '.databases.action_log' .ratchet/notion-config.json)
ACTION_LOG_DS=$(jq -r '.data_sources.action_logs' .ratchet/notion-config.json)
DOCS_DB=$(jq -r '.databases.documentation' .ratchet/notion-config.json)
DOCS_DS=$(jq -r '.data_sources.documentation' .ratchet/notion-config.json)
PROJECTS_DB=$(jq -r '.databases.projects' .ratchet/notion-config.json)
PROJECTS_DS=$(jq -r '.data_sources.projects' .ratchet/notion-config.json)
CLIENTS_DB=$(jq -r '.databases.clients' .ratchet/notion-config.json)
CLIENTS_DS=$(jq -r '.data_sources.clients' .ratchet/notion-config.json)
```

## Create

### Create Task

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "parent": {"database_id": "${TASKS_DB}"},
  "properties": {
    "Task name": {"title": [{"text": {"content": "Wire Ratchet plugin into bridge"}}]},
    "Project": {"relation": [{"id": "${PROJECT_PAGE_ID}"}]},
    "Status": {"status": {"name": "Ready"}},
    "Priority": {"select": {"name": "Medium"}},
    "Effort Level": {"select": {"name": "Small"}},
    "Due date": {"date": {"start": "2026-05-15"}}
  }
}
EOF
```

### Create Action Log

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "parent": {"database_id": "${ACTION_LOG_DB}"},
  "properties": {
    "Entry": {"title": [{"text": {"content": "Migrated openclaw secrets to Doppler"}}]},
    "Date": {"date": {"start": "2026-04-28"}},
    "Author": {"select": {"name": "Forge"}},
    "Type": {"select": {"name": "Action"}},
    "Project": {"relation": [{"id": "${PROJECT_PAGE_ID}"}]}
  },
  "children": [
    {
      "object": "block", "type": "paragraph",
      "paragraph": {"rich_text": [{"text": {"content": "Optional longer description."}}]}
    }
  ]
}
EOF
```

### Create Documentation

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "parent": {"database_id": "${DOCS_DB}"},
  "properties": {
    "Request": {"title": [{"text": {"content": "How to install the Ratchet plugin"}}]},
    "Status": {"status": {"name": "Draft"}},
    "Projects": {"relation": [{"id": "${PROJECT_PAGE_ID}"}]}
  }
}
EOF
```

### Create Project

```bash
curl -s -X POST "https://api.notion.com/v1/pages" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "parent": {"database_id": "${PROJECTS_DB}"},
  "properties": {
    "Project name": {"title": [{"text": {"content": "New Initiative"}}]},
    "Client": {"relation": [{"id": "${CLIENT_PAGE_ID}"}]},
    "Status": {"status": {"name": "In Progress"}}
  }
}
EOF
```

## Query

### Query Tasks (active only, by project)

```bash
curl -s -X POST "https://api.notion.com/v1/data_sources/${TASKS_DS}/query" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "filter": {
    "and": [
      {"property": "Project", "relation": {"contains": "${PROJECT_PAGE_ID}"}},
      {"property": "Status", "status": {"does_not_equal": "Done"}},
      {"property": "Status", "status": {"does_not_equal": "Canceled"}}
    ]
  },
  "sorts": [{"property": "Due date", "direction": "ascending"}],
  "page_size": 25
}
EOF
```

### Query Action Log (last 7 days)

```bash
SINCE=$(date -u -d '7 days ago' +%Y-%m-%d 2>/dev/null || date -u -v-7d +%Y-%m-%d)

curl -s -X POST "https://api.notion.com/v1/data_sources/${ACTION_LOG_DS}/query" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "filter": {
    "property": "Date",
    "date": {"on_or_after": "${SINCE}"}
  },
  "sorts": [{"property": "Date", "direction": "descending"}],
  "page_size": 50
}
EOF
```

### Query Projects (active)

```bash
curl -s -X POST "https://api.notion.com/v1/data_sources/${PROJECTS_DS}/query" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "filter": {"property": "Status", "status": {"does_not_equal": "Archived"}},
  "page_size": 100
}
EOF
```

### Query Clients (active)

```bash
curl -s -X POST "https://api.notion.com/v1/data_sources/${CLIENTS_DS}/query" \
  -H "$NOTION_AUTH" -H "$NOTION_VER" -H "Content-Type: application/json" \
  -d @- <<EOF
{
  "filter": {"property": "Status", "status": {"equals": "Active"}},
  "page_size": 100
}
EOF
```

## Notes

- Always prefer `curl` over the Notion MCP for create operations — the MCP `post-page` double-serializes nested JSON, breaking many properties silently.
- For updates (PATCH `/v1/pages/{page_id}`), the same property shape applies.
- Property *names* in the JSON above are the canonical Ratchet names. If a consumer has renamed properties (see `docs/customization.md`), pull the actual names from `notion-config.json` `properties` and substitute.
