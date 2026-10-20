# Liquibase Migration Standards

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![version 1.1.0](https://img.shields.io/badge/version-1.1.0-blue.svg)](CHANGELOG.md)
[![jesusarielgb-works](https://img.shields.io/badge/org-jesusarielgb--works-blue)](https://github.com/jesusarielgb-works)

> Naming conventions, structure, rollback strategy and verification for database migrations
> using Liquibase — the single source of Liquibase mechanics for every jesusarielgb-works framework.

**Author:** [Jesús Ariel González Bonilla](https://github.com/jesusarielgb-works)

**For:** teams that version a relational schema with Liquibase 4.x, in one deployable or many.
The [monolith](https://github.com/jesusarielgb-works/monolith-governance-framework) and
[microservices](https://github.com/jesusarielgb-works/microservices-governance-framework)
frameworks set the policy (one sequence, two-phase drops); this standard sets the mechanics.

## Sections

| Doc | Content |
|-----|---------|
| [01 - Naming Conventions](docs/01-naming-conventions.md) | Changeset IDs, file names, author rules |
| [02 - Migration Structure](docs/02-migration-structure.md) | Changelog hierarchy, preconditions |
| [03 - Rollback Strategy](docs/03-rollback-strategy.md) | Rollback blocks, forbidden operations |
| [04 - Verification](docs/04-verification.md) | The command that checks each rule, CI job |

## Examples

See [examples/](examples/) for ready-to-use XML changesets.

## Evidence

Rules cite verified records of the
[engineering casebook](https://github.com/jesusarielgb-works/engineering-casebook) as `[CASE-NNN]`.

## How to Cite

> González Bonilla, J. A. (2026). *Liquibase Migration Standards* (v1.1.0). jesusarielgb-works. https://github.com/jesusarielgb-works/liquibase-migration-standards

See also [`CITATION.cff`](CITATION.cff).

## License

MIT — Jesús Ariel González Bonilla. See [LICENSE](LICENSE).
