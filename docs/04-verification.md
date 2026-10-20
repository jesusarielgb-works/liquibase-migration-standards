# 04 — Verification

Every rule in this standard is checked by a command. Run them in CI on every pull request
that touches `db/changelog/`, against a throwaway database.

| What | Command | Fails when |
|---|---|---|
| Changelog is well formed, no ran changeset was edited | `liquibase validate` | XML error, duplicate ID, checksum mismatch |
| Every changeset rolls back and re-applies | `liquibase update-testing-rollback` | A changeset has no working rollback |
| No empty rollback | `grep -rE '<rollback\s*/>' db/changelog` | Any match |
| IDs are lowercase with hyphens | `grep -rE 'id="[^"]*[A-Z_][^"]*"' db/changelog` | Any match |
| Forward changesets do not drop what the same release stops using | Reviewer check: a `dropTable` / `dropColumn` outside `<rollback>` names the earlier release that stopped using it | — |

## Example job (GitHub Actions, PostgreSQL)

```yaml
jobs:
  migrations:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16
        env: { POSTGRES_PASSWORD: ci, POSTGRES_DB: ci }
        ports: ['5432:5432']
    steps:
      - uses: actions/checkout@v4
      - name: Validate and test rollback
        run: |
          docker run --rm --network host -v "$PWD:/liquibase/changelog" liquibase/liquibase:4.29 \
            --url=jdbc:postgresql://localhost:5432/ci --username=postgres --password=ci \
            --changelog-file=db/changelog/db.changelog-master.xml \
            validate
          docker run --rm --network host -v "$PWD:/liquibase/changelog" liquibase/liquibase:4.29 \
            --url=jdbc:postgresql://localhost:5432/ci --username=postgres --password=ci \
            --changelog-file=db/changelog/db.changelog-master.xml \
            update-testing-rollback
```

Pin the image to the Liquibase version the project's build resolves, and re-check it when
that version changes.
