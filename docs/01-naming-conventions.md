# 01 — Naming Conventions

## Changeset ID Format

```
YYYY-MM-DD-NNN-short-description
```

Examples:
- `2026-01-15-001-create-users-table`
- `2026-01-15-002-add-email-to-users`

## Rules
- IDs are permanent — never change a committed ID
- NNN resets per date (001, 002, 003...)
- Lowercase, hyphens only — no underscores in ID
- Author: GitHub username — e.g. `jesusarielgb`
- Verb + object: `create-users-table`, `add-email-column`, `drop-legacy-index`

## File Naming

```
YYYY-MM-DD-NNN-description.xml
```

## Master Changelog

```xml
<databaseChangeLog xmlns="http://www.liquibase.org/xml/ns/dbchangelog" ...>
  <include file="db/changelog/2026/2026-01-15-001-create-users-table.xml"/>
  <include file="db/changelog/2026/2026-01-15-002-add-email-to-users.xml"/>
</databaseChangeLog>
```
