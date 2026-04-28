# Customization

The Ratchet skills depend on property *roles*, not property *names*. This means you can rename properties in Notion, add fields, or extend select options — as long as you keep the data contract aligned.

## What you can change

### Rename properties

Notion property names are display strings — they don't have to match the canonical Ratchet names. If you've renamed `Task name` to `Headline` (because that fits your team's vocabulary better), tell the skill via `properties` in `.ratchet/notion-config.json`:

```json
{
  "properties": {
    "tasks": {
      "title": "Headline"
    },
    "action_log": {
      "title": "Entry",
      "author": "Logged by"
    }
  }
}
```

Roles the skill recognizes:

| DB | Roles | Required type |
|---|---|---|
| `tasks` | `title`, `project`, `status`, `priority`, `effort_level`, `due_date`, `assignee` | title, relation, status, select, select, date, people |
| `action_log` | `title`, `date`, `author`, `type`, `project`, `summary` | title, date, select, select, relation, rich_text |
| `documentation` | `title`, `status`, `projects`, `platform`, `tutorial_url` | title, status, relation, select, url |
| `projects` | `title`, `client`, `parent`, `status`, `priority` | title, relation, relation, status, select |
| `clients` | `title`, `status` | title, status |

### Add select options

The baseline ships with conservative option sets (e.g., Status = Needs Definition / Blocked / Ready / In Progress / Done / Canceled). Adding new options in Notion does not break the skill. The skill does not enforce a closed set — it passes whatever the user asks through.

### Add new properties

Add freely. The skill only writes the properties it knows about; extras are left untouched.

### Add new authors

Author is a `select` field. Add team members, other agents, etc. The skill accepts any author name passed in.

## What you should NOT change

### Property *types*

If you change `Status` from `status` to `select`, queries that use `status` filters will silently fail. Keep types as documented in the role table above.

### Database *count*

The skills assume five databases. Splitting Tasks into "Personal Tasks" and "Work Tasks" requires forking the skill — the contract doesn't accommodate it.

### Data source IDs in `notion-config.json`

These are looked up by key (e.g., `data_sources.tasks`). Don't rename the keys — only the *values* (the UUIDs themselves) can change.

## Migration after a rename

If you rename a property in Notion *after* the skill has been used:

1. Update `.ratchet/notion-config.json` `properties` map to point the role at the new name.
2. Existing pages keep working (Notion preserves property identity through renames).
3. Test by creating one new record per affected role — confirm it lands in the right field.

## Migration after adding a property

No skill change needed. The new property exists in Notion; the skill ignores it on writes (unknown properties are not sent in payloads). To start writing to the new property, either extend the skill (PR to this plugin) or write to it directly via curl in a consumer-side script.

## Migration after deleting a property

If you delete a role-bound property (e.g., remove `Priority` from Tasks), the skill will fail when it tries to set that property. Either:

- Restore the property in Notion (the deleted property's history is recoverable via Notion's trash for 30 days), or
- Override the role to a still-existing property of the same type, or
- Set the role's value to `null` in `.ratchet/notion-config.json` `properties` to tell the skill "skip this role." (Future enhancement; not yet implemented.)

## Anti-pattern: forking the schema heavily

If you find yourself overriding more than 3-4 property names, or restructuring the relations, you've drifted far enough that the plugin's skills won't match your workflow well. At that point, fork the plugin or build your own skill. The plugin's value is in the *common case*; bespoke setups are not its target.
