# Ratchet Framework

The doctrinal core: vocabulary and design principles every Ratchet-aware system inherits.

## What Ratchet is

A productivity framework for agent-assisted system building and knowledge work. Built around five Notion databases that hold the durable state of a person's (or company's) operations:

| Database | Purpose |
|---|---|
| **Projects** | The unit of effort. Has a Client, a Status, optionally a Parent Project. |
| **Tasks** | Discrete work items belonging to a Project. |
| **Action Log** | Append-only record of significant actions and decisions. Authored by a human or an agent. |
| **Documentation** | Persistent knowledge artifacts: how-tos, decisions, references. Linked to Projects. |
| **Clients** | Who pays / who the work is for. |

Every other Ratchet primitive composes from these five.

## Primitives

The full primitive set Ratchet recognizes:

- **Action Log** — a logged action or decision, with author, date, optional project link.
- **Time Entry** — a duration of work tied to a Project (lives in Toggl/Replicon today; not yet a Notion DB).
- **Project** — see above.
- **Task** — see above.
- **Documentation** — see above.
- **Proposal** — a project's pre-engagement form (estimates, scope), upgrades to a Project on acceptance.
- **Meeting** — a synchronous interaction; its notes typically generate Tasks and Action Log entries.
- **Scope** — the boundary statement for a Project; what's in, what's out.

Not every primitive has a dedicated database yet. The core five (Projects, Tasks, Action Log, Documentation, Clients) are the load-bearing ones; Proposals and Meetings can live as their own DBs in larger workspaces or as pages-in-a-Project for smaller ones.

## Contexts

Ratchet recognizes seven *contexts* — modes of work the user (or agent) is in. They're not statuses on a record; they're shorthand for "what kind of session is this":

| Context | What it means |
|---|---|
| **Deep Thought** | Focused thinking on a single hard problem. Output: a decision, a plan, or a question to investigate. |
| **Filing** | Putting things in their right place. Cleanup, taxonomy, tagging, archival. |
| **Research** | Reading / studying to answer a specific question or build a model of something. |
| **Prototyping** | Building a throwaway to learn from. Code, doc, or sketch. |
| **Sparks** | Capturing inbound ideas, observations, or items to revisit. Fast in, no resolution required. |
| **Question** | A held-open inquiry. Not yet a task; not yet a research thread. |
| **Harvester** | Reaping outputs from earlier contexts — converting Sparks/Questions/Research into Tasks, Documentation, or Project changes. |

Skills should let the user (or agent) declare a context when starting a session, and shape their behavior accordingly. A `Sparks` session optimizes for low-friction capture. A `Harvester` session looks at recent Sparks and asks "what becomes a Task now?"

## Design Principles

These are not Ratchet-specific in origin, but Ratchet treats them as load-bearing.

### Composable building blocks, not duplication (the "triangle")

When knowledge or capability is shared across multiple skills (API/tool access, philosophy components, utilities like date handling), extract it to a shared module. Skills compose from blocks in a triangle pattern:

```
Skill A → Block ← Skill B
```

Never chain skills through each other for shared logic (`Skill A → Skill B for a lookup that B happens to have`). If you find yourself copy-pasting between skills, or writing "see other-skill for how to do X," stop and extract a block.

### Subtract first (Klotz)

When solving a problem, don't default to addition. A simpler, more elegant, or equally effective solution often lies in *removing* something rather than adding something new. Named after Leidy Klotz's *Subtract: The Untapped Science of Less* (2021), which documents the human bias toward additive change.

Applies anywhere scope expands: new skills, new plan deliverables, new data fields, action items surfaced by a triage, new Notion properties. Before asking "what should we add?", ask "what could we remove?"

A Deep Study pilot that produces 20 action items and results in 20 additions is a failure mode.

### No hidden memory

The system gets smarter over time through deliberate, observable accretion — never through hidden state. Everything worth persisting across sessions is version-controlled and visible.

Claude Code's harness offers an offloaded memory system at `~/.claude/projects/.../memory/`. Ratchet-aware projects **do not use it.** When the harness prompts you to save a memory, propose a version-controlled home instead (CLAUDE.md, docs, plans, context files). Memory files outside git are invisible to diffs and review, and let quality decay silently.

This principle extends to anything that survives across sessions: prefer a file the user can grep, diff, and PR against over an opaque store the agent reaches into.
