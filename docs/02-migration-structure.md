# 02 — Migration Structure

## Folder Layout

```
src/main/resources/
+-- db/
    +-- changelog/
    |   +-- db.changelog-master.xml
    |   +-- 2026/
    |       +-- 2026-01-15-001-create-users-table.xml
    |       +-- 2026-01-15-002-add-email-to-users.xml
    +-- data/
        +-- 2026-01-15-seed-roles.xml  (reference data, separated from schema)
```

Spring Boot reads `classpath:db/changelog/db.changelog-master.yaml` by default; with an XML
master, set `spring.liquibase.change-log: classpath:db/changelog/db.changelog-master.xml`
([Spring Boot reference](https://docs.spring.io/spring-boot/how-to/data-initialization.html)).
A framework may fix another folder — the monolith framework uses `db/migration/` and wires
`spring.liquibase.change-log` to it — and a project that runs migrations from a separate
module or pipeline keeps the same tree there. The rules in this standard do not change.

## Changeset Template

```xml
<changeSet id="2026-01-15-001-create-users-table" author="ariel5253">
    <preConditions onFail="MARK_RAN">
        <not><tableExists tableName="users"/></not>
    </preConditions>
    <createTable tableName="users">
        <column name="id" type="BIGSERIAL">
            <constraints primaryKey="true" nullable="false"/>
        </column>
        <column name="created_at" type="TIMESTAMP" defaultValueComputed="NOW()">
            <constraints nullable="false"/>
        </column>
    </createTable>
    <rollback><dropTable tableName="users"/></rollback>
</changeSet>
```

## Preconditions: choose `onFail` deliberately

| `onFail` | Use it when | Risk |
|---|---|---|
| `MARK_RAN` | The changeset only creates something, and its absence is the precondition (`not tableExists`) | The changeset is recorded as applied without running. If the existing object differs from what the changeset would create, nothing reports it |
| `HALT` (Liquibase's default) | Anything that alters or moves existing data | The deploy stops — which is the point |

When `MARK_RAN` is used, a schema diff in CI (`liquibase diff` against a database built from
scratch) is what proves the existing object matches.
