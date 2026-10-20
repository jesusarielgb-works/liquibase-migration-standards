# 01 — Naming Conventions

## Changeset ID Format

```
YYYY-MM-DD-NNN-[module-]verb-object
```

Examples:
- `2026-01-15-001-create-users-table`
- `2026-01-15-002-add-email-to-users`
- `2026-01-15-003-billing-add-invoice-lines` — in a modular monolith the owning module follows `NNN`

## Rules

| Rule | How it is verified |
|---|---|
| IDs are permanent — never change a committed ID or the content of a changeset that ran anywhere | Liquibase stores a checksum per changeset; `liquibase validate` fails when a ran changeset was edited |
| `NNN` restarts at `001` for each date | Reviewer check on the file list of the PR |
| Lowercase and hyphens only — no underscores in the ID | `grep -E 'id="[^"]*[A-Z_][^"]*"' -r db/changelog` returns nothing |
| `author` is the committer's GitHub username — e.g. `ariel5253` | Reviewer check: matches the PR author |
| The description is verb + object: `create-users-table`, `add-email-column`, `drop-legacy-index` | Reviewer check |
| In a modular monolith, the module segment names the module that owns the table | Reviewer check against the module catalog |

## File Naming

One changeset per file, named exactly like its ID:

```
YYYY-MM-DD-NNN-[module-]verb-object.xml
```

See [`../examples/`](../examples/) — both files follow this rule.

## Master Changelog

```xml
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog" ...>
  <include file="db/changelog/2026/2026-01-15-001-create-users-table.xml"/>
  <include file="db/changelog/2026/2026-01-15-002-add-email-to-users.xml"/>
</databaseChangeLog>
```

One master changelog per database — never one per module — so the whole database has a
single ordered history.
