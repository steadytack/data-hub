# 06 Data model

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-06 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

Everything lives in one Postgres 17 database with PostGIS 3.5, in five schemas: `bronze` (raw loads), `silver` (cleaned), `gold` (published views and metadata tables, the only schema consumers reach), `catalogue` (products, registers, runs, users) and `audit` (audit and usage events). Airflow's own metadata is in a separate database (CMP-007). Keys are UUIDv7 except small lookup tables.

## Entities
| DATA | Entity | Purpose | FRs | Personal data | Retention |
|---|---|---|---|---|---|
| DATA-001 | Product | One data product: name, owner, deputy, domain, authority and basis, access level, AI use, data origin, licence, schedule, current tier | FR-002, FR-006, FR-069 | No (owner is a user reference) | Kept until an admin retires it |
| DATA-002 | Product version | A published (or promotion-ready) version: number, schema hash, dates, row count, the bronze version it came from | FR-107, FR-032 | No | Kept with the product |
| DATA-003 | Manifest | The loaded manifest's text and parsed fields, with its Git path and commit | FR-002, FR-003 | No | Every loaded revision kept |
| DATA-004 | Licence | A licence register entry | FR-004, FR-005, FR-070 | No | Kept |
| DATA-005 | Known issue | A register entry with scope, type, severity, measure, origin, status, owner and fix link | FR-007, FR-022, FR-058 | No (reporter is a user reference) | Kept; resolved entries stay visible |
| DATA-006 | Check result | One check's outcome at one boundary in one run, with the measured value | FR-021 | No | Kept with the run |
| DATA-007 | Run and step | A pipeline run (trigger, status, timings, skip reason) and its steps (status, duration, log) | FR-041, FR-042, FR-009 | No | Kept 24 months; step logs 12 months |
| DATA-008 | Column profile | Per version and column: type, minimum, maximum, null rate, distinct count; per table: row count, geometry type, coordinate system, extent | FR-025, FR-026 | No | Kept with the version |
| DATA-009 | Trust score | Per version: each signal's value and share, the total and the proposed tier | FR-028, FR-029 | No | Kept with the version |
| DATA-010 | Tier decision | An override (with reason), sign-off, restore or confirmation | FR-030, FR-031, FR-033 | No (actor is a user reference) | Kept |
| DATA-011 | User | Named account: name, email, password hash, role, active flag | FR-066, FR-067 | Yes: name, email | Until an admin deletes the account; then removed, with audit events keeping only the user ID until purged |
| DATA-012 | Group | An admin-managed group | FR-068 | No | Until deleted |
| DATA-013 | Group member | A user in a group | FR-068 | Yes: links a person to a group | Removed with the membership or the user |
| DATA-014 | Named grant | Access to one product for one user, with an optional expiry | FR-069 | Yes: links a person to a product | Removed on revoke; expired grants deleted after 30 days |
| DATA-015 | Audit event | Who did what to which item, when | FR-008, FR-077 | Yes: user ID | 12 months, then purged (NFR-009) |
| DATA-016 | Usage event | Who pulled which product through which door, when, how many rows | FR-076, FR-086 | Yes: user ID | 12 months, then purged (NFR-009) |
| DATA-017 | Bronze load | A loaded file's record (hash, size, link, version text, load time) and its feature table, unchanged | FR-010, FR-011, FR-013 | No (public or synthetic) | Last four per source (RULE-002) |
| DATA-018 | Silver table | Cleaned, typed product data with standard key and repair flag | FR-018 | No | Rebuilt each run |
| DATA-019 | Gold view and metadata tables | The published view with quality flags, and the metadata tables beside it | FR-019, FR-020 | No | Current published version |
| DATA-020 | Personal access token | Hashed token, name, expiry, last used | FR-078 | Yes: tied to a user | Deleted on revoke or 30 days after expiry |
| DATA-021 | Dictionary entry | Per column: definition, unit, source of the text (harvested or owner) | FR-054, FR-060, FR-027 | No | Kept with the product |
| DATA-022 | Read-only login | A user's Postgres login and its grants, kept in step with access | FR-072 | Yes: named after the user | Dropped with the user or on deactivation |
| DATA-023 | Data request (Phase 2) | A request for a dataset, its matches and triage | FR-062 | Yes: requester ID | Kept 24 months after closing |
| DATA-024 | Usefulness rating (Phase 2) | Yes or no and a comment per product | FR-063 | Yes: rater ID | Kept 24 months |
| DATA-025 | Subscription (Phase 3) | A user's subscription to a product | FR-102 | Yes: subscriber ID | Until unsubscribed or the user is deleted |
| DATA-026 | Access request (Phase 3) | A request for access to a restricted product, and its outcome | FR-103 | Yes: requester ID | Kept 12 months after closing |
| DATA-027 | Lineage edge (Phase 3) | Upstream and downstream products from OpenLineage events | FR-104 | No | Kept |
| DATA-028 | Use case (Phase 3) | A named use case and its minimum tier | FR-091 | No | Kept |
| DATA-029 | Domain | A domain's name, steward and deputy | FR-116, FR-117 | Yes: steward and deputy user IDs | Kept |
| DATA-030 | Connection | A named source connection: type, address, account name and a secret reference (never the secret) | FR-121, FR-123, FR-017, FR-015 | No | Kept; deleted with its last product |
| DATA-031 | Door setting | Whether each door is enabled, globally and per product | FR-127 | No | Kept |

## Fields
Main fields; the rest follow the template's conventions (`id`, `created_at`, `updated_at`).

| Field | Entity | Type | Required | Rule |
|---|---|---|---|---|
| name | DATA-001 | text | yes | Unique slug from the manifest |
| owner_id, deputy_id | DATA-001 | user reference | owner yes; deputy from Phase 3 | Owner holds the owner role |
| authority, authority_basis | DATA-001 | enum, text | authority yes | authoritative record, reference copy, derived copy, third-party |
| access_level | DATA-001 | enum | yes | Open, Group or Named; default Named (RULE-008) |
| ai_use, data_origin | DATA-001 | boolean, enum | yes | ai_use true only for public or synthetic (RULE-001) |
| licence_id | DATA-001 | licence reference | yes | Must exist (FR-005) |
| tier, tier_reason | DATA-001 | enum, text | yes | Certified, Usable with caveats, Exploratory only, Not for AI (RULE-006) |
| number | DATA-002 | text | yes | major.minor (RULE-013) |
| created_on, revised_on, published_on, last_run_at, next_expected_on | DATA-002 | dates | published_on once promoted | Kept separately (FR-023) |
| freshness | DATA-002 | enum | yes | Fresh, Late, Stale, Unknown (RULE-004) |
| attribution, ceiling, ai_permitted | DATA-004 | text, enum, boolean | yes | Ceiling per RULE-012 |
| severity, status, origin | DATA-005 | enums | yes | FR-007's values |
| measure_rule, measure_value | DATA-005 | text, number | for automatic issues | From the failing check (RULE-014) |
| rule, boundary, severity, passed, value | DATA-006 | text, enum, enum, boolean, number | yes | boundary: bronze-silver or silver-gold |
| sha256, size_bytes, link, version_text, loaded_at | DATA-017 | text, integer, text, text, timestamp | yes | Hash of the downloaded file |
| geometry_repaired | DATA-018 | boolean | yes | True where repair changed the geometry |
| email | DATA-011 | text | yes | Unique, lower-cased |
| password_hash | DATA-011 | text | until Phase 3 | bcrypt |
| expires_on | DATA-014 | date | no | RULE-016 |
| token_hash, expires_at | DATA-020 | text, timestamp | yes | At most 90 days (SEC-008) |
| actor_id, action, item, at | DATA-015 | uuid, text, text, timestamp | yes | No query results (FR-008) |
| door, rows | DATA-016 | enum, integer | yes | product page, sql, mcp, export (later ogc, rest, odata) |

## Relationships
| REL | From | To | Cardinality | Rule |
|---|---|---|---|---|
| REL-001 | DATA-001 | DATA-002 | one to many | A product has versions; one is current |
| REL-002 | DATA-001 | DATA-004 | many to one | Every product links one licence |
| REL-003 | DATA-001 | DATA-005 | one to many | Issues belong to a product (and optionally versions) |
| REL-004 | DATA-002 | DATA-006 | one to many | Check results per version's runs |
| REL-005 | DATA-001 | DATA-007 | one to many | Runs per product |
| REL-006 | DATA-002 | DATA-008 | one to many | Profiles per version |
| REL-007 | DATA-012 | DATA-013 | one to many | Groups have members (DATA-011) |
| REL-008 | DATA-001 | DATA-014 | one to many | Named grants per product, each for one DATA-011 user |
| REL-009 | DATA-001 | DATA-012 | many to many | Groups chosen for a Group-level product |
| REL-010 | DATA-002 | DATA-017 | many to one | A version is built from one bronze load |
| REL-011 | DATA-011 | DATA-020 | one to many | Users own tokens |
| REL-012 | DATA-001 | DATA-021 | one to many | Dictionary entries per product column |

## Ownership, flows and source of truth
- **Manifests and dbt models:** Git (CMP-020) is the source of truth; the catalogue keeps each loaded revision (DATA-003).
- **Trust:** the catalogue is the source of truth for tier, issues and checks; Airflow only produces the evidence (source, Reporting run results).
- **Flow:** source → bronze (unchanged) → silver (dbt) → gold view (dbt) → doors; checks at each boundary; profiles, scores and issues written to the catalogue; metadata tables in `gold` mirror the catalogue for the SQL door.
- **People and access:** users, groups and grants in the catalogue; read-only logins derived from them (DATA-022).

## Retention, deletion and migration
- Bronze keeps four versions (RULE-002); audit and usage events 12 months (NFR-009); everything else as in the entity table.
- Deleting a user removes their account, memberships, grants, tokens and login; audit and usage events keep the bare user ID until purged.
- Migrations are Alembic, reversible where practical; the first creates the five schemas (FR-001). No data migrates from elsewhere.
- Classification: product data in Phases 0 and 1 is public (OGL) or synthetic; user accounts are the only personal data.
