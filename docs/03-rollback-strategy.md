# 03 — Rollback Strategy

## Rules
1. Every changeset MUST include a `<rollback>` block.
2. Rollbacks must be tested in staging before production.
3. Never use `<rollback/>` (empty) — Liquibase cannot roll back without instructions.

## Forbidden Operations
- `DROP TABLE` — use rename + archive pattern, drop in a later release
- `DROP COLUMN` — mark deprecated first, drop in next major version
- Irreversible data transforms without a prior backup step

## Rollback Commands

```bash
liquibase rollbackCount N           # roll back last N changesets
liquibase rollback <tag>            # roll back to tag
liquibase rollbackToDate YYYY-MM-DD
```

## Tagging Before Production Deploy

```bash
liquibase tag release-1.0.0
```
