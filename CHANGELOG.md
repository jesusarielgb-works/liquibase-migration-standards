# Changelog
Format: [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) | [SemVer](https://semver.org/)
## [Unreleased]
## [1.1.0] - 2026-10-20
### Added
- `docs/04-verification.md`: the command that checks each rule, and an example CI job
  running `validate` and `update-testing-rollback` on an ephemeral database
- "How it is verified" column for every rule
- Optional module segment in the changeset ID for modular monoliths
- Guidance on choosing `onFail` (`MARK_RAN` versus `HALT`)
- Evidence: [CASE-005] from the engineering casebook backs the two-phase drop rule
### Changed
- Example files renamed to the documented rule: `2026-01-15-001-create-users-table.xml`,
  `2026-01-15-002-add-email-to-users.xml` (were `V001__…`, `V002__…`)
- `author` is the GitHub username `ariel5253`
- Forbidden operations now say they apply to forward changesets, not to rollback blocks
- Master changelog renamed to `db.changelog-master.xml`, with the Spring Boot wiring
## [1.0.0] - 2026-08-24
### Added
- Initial standards: naming conventions, migration structure, rollback strategy, examples
