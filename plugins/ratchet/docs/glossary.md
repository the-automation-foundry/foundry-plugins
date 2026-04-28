# Ratchet Glossary

Terms used inside the Ratchet framework. This is the *baseline* vocabulary — consumers extend it in their own repos with project- or company-specific terms.

## Core terms

| Term | Definition |
|---|---|
| **Ratchet** | This framework. The five-DB Notion structure plus the methodology around it. |
| **Database** | One of the five canonical Notion DBs: Projects, Tasks, Action Log, Documentation, Clients. |
| **Data source** | Notion's term for a queryable collection inside a database. Each Ratchet database has exactly one data source. Use data source IDs for queries, database IDs for page creation. |
| **Primitive** | A canonical Ratchet record type — Project, Task, Action Log, Documentation, Client, plus Proposal, Meeting, Time Entry, Scope. |
| **Context** | A mode of work: Deep Thought, Filing, Research, Prototyping, Sparks, Question, Harvester. See [framework.md](./framework.md). |
| **Author** | The agent or human responsible for an Action Log entry. Conventionally `Nick` (user) or `Forge` (agent on the Foundry's OpenClaw deployment); consumers may add their own author names. |

## Action Log conventions

| Term | Definition |
|---|---|
| **Action** | An Action Log Type. A thing that was done. |
| **Decision** | An Action Log Type. A choice that was made. Distinct from Action because decisions carry forward as constraints; actions are point-in-time. |
| **Proactive logging** | Author=Forge entries the agent writes without being prompted, for config changes / deployments / significant decisions. See the `ratchet` skill's "Proactive logging guidelines" section. |

## Plugin / consumer terms

| Term | Definition |
|---|---|
| **Consumer** | A repo that installs and uses the Ratchet plugin (e.g., `foundry-ops-bridge`, `openclaw-gcp`). |
| **Canonical config path** | `.ratchet/notion-config.json` relative to the agent's working directory. The single path Ratchet skills read for database IDs. |
| **Data contract** | The shape of `notion-config.json` (and optional auxiliary files). See [data-contract.md](./data-contract.md). |
| **Property role** | The function a Notion property serves (e.g., "the date a task is due"). Consumers may rename properties as long as roles and types are preserved. See [customization.md](./customization.md). |

## What this glossary does NOT cover

- **Consumer-specific vocabulary** — your clients, projects, teams, tools. That belongs in your repo's own glossary or on the relevant Notion page.
- **Ratchet workspace metadata** — DB IDs, project lists, client names. Those live in the consumer's `.ratchet/notion-config.json` and operational data files, not here.
