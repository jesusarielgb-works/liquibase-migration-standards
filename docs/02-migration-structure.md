# 02 — Migration Structure

## Folder Layout

```
src/main/resources/
+-- db/
    +-- changelog/
    |   +-- master.xml
    |   +-- 2026/
    |       +-- 2026-01-15-001-create-users-table.xml
    |       +-- 2026-01-15-002-add-email-to-users.xml
    +-- data/
        +-- 2026-01-15-seed-roles.xml  (reference data, separated from schema)
```

## Changeset Template

```xml
<changeSet id="2026-01-15-001-create-users-table" author="jesusarielgb">
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
