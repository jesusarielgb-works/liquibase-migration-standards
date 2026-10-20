# 03 — Rollback Strategy

## Rules

| Rule | How it is verified |
|---|---|
| Every changeset includes a `<rollback>` block | `liquibase update-testing-rollback` in CI fails on a changeset it cannot roll back — see [`04-verification.md`](04-verification.md) |
| Never an empty `<rollback/>` — Liquibase cannot roll back without instructions | `grep -rE '<rollback\s*/>' db/changelog` returns nothing |
| Rollbacks are exercised before production | The same CI job runs update → rollback → update on an ephemeral database for every pull request |

## Forbidden Operations

These apply to **forward** changesets. A rollback block may drop what its own changeset
created — that is how a `createTable` is undone.

| Forbidden in a forward changeset | Instead | Evidence |
|---|---|---|
| `DROP TABLE` in the release that stops using the table | Stop using it in release N, rename and archive it; drop it in release N+1 | [CASE-005] |
| `DROP COLUMN` in the release that stops using the column | Stop reading and writing it in release N; drop it in release N+1 | [CASE-005] |
| An irreversible data transform without a backup step | A preceding changeset copies the affected rows to an archive table | — |

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

---

**Evidence:** [CASE-005](https://github.com/jesusarielgb-works/engineering-casebook/blob/v1.0.0/records/cases/CASE-005-stripe-online-migrations-2017.md)
— Stripe moved hundreds of millions of records by adding the new shape first and removing
the old one last.
