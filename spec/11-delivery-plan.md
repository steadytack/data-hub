# 11 Delivery plan

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-11 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

Epics in build order. Each is independently testable and states what it depends on. The source's eleven epics span phases, so they are split here by phase (DEF-16: milestones of coherent epics). Phase 0 follows Build Brief 001 slice by slice: one slice per branch, its tests first, and the next slice only once the current one passes (brief, How to use this brief). Each phase ends at its own gate (scope, Phases and gates); Tier 3 applies from EPIC-010 (D-008).

## Epics
| EPIC | Epic | Outcome | FRs | Stories | Components | Release | Depends on |
|---|---|---|---|---|---|---|---|
| EPIC-001 | Foundation (slice 0) | The stack starts and the tests run with one command; five schemas exist | FR-001 | US-001 | CMP-001, CMP-002, CMP-003, CMP-006, CMP-007, CMP-008, CMP-019 | Phase 0 | none |
| EPIC-002 | Catalogue tables, manifest and registers (slice 1) | A product registers from a manifest; bad manifests are refused; licences, issues and audit are recorded | FR-002 to FR-008 | US-002 to US-006 | CMP-001, CMP-003, CMP-004, CMP-020 | Phase 0 | EPIC-001 |
| EPIC-003 | Ingest to bronze and silver (slices 2 and 3) | The flood map lands in bronze unchanged and versioned, and builds into a clean silver table | FR-009, FR-010, FR-011, FR-018 | US-007 to US-010 | CMP-004, CMP-005, CMP-010, CMP-003 | Phase 0 | EPIC-002 |
| EPIC-004 | Checks, profiling, gold and trust (slices 4 to 6) | Failures are caught and visible; a published view with metadata; a tier; only passing products promote | FR-019 to FR-026, FR-028 to FR-033, FR-107 | US-011 to US-021 | CMP-004, CMP-005, CMP-001, CMP-003 | Phase 0 | EPIC-003 |
| EPIC-005 | DAG factory and embedded Airflow (slice 7) | The pipeline runs from the manifest on a schedule with run history in the app | FR-038 to FR-043 | US-022, US-023, US-024 | CMP-004, CMP-006, CMP-007, CMP-001, CMP-002 | Phase 0 | EPIC-004 |
| EPIC-006 | Sign-in, catalogue and product page (slice 8) | People sign in, find products and see why to trust them; access holds on every door | FR-050 to FR-060, FR-066 to FR-069, FR-071, FR-072, FR-073, FR-078 | US-025 to US-035 | CMP-001, CMP-002, CMP-003 | Phase 0 | EPIC-005 |
| EPIC-007 | Second product and a spatial join (slice 9) | Two products combine, and so do their caveats | FR-108, FR-109 | US-036, US-037 | CMP-004, CMP-005, CMP-003 | Phase 0 | EPIC-006 |
| EPIC-008 | MCP server and file export (slices 10 and 11) | AI tools and files get the data with its caveats, within limits and access | FR-074, FR-082 to FR-087, FR-094, FR-095, FR-096 | US-038 to US-043 | CMP-008, CMP-001, CMP-010 | Phase 0 | EPIC-007 |
| EPIC-009 | End-to-end demo and findings (slice 12) | The demo script passes on a clean stack and the findings note is written | FR-109, FR-022, FR-031, FR-084 | US-044 | CMP-001, CMP-002, CMP-006, CMP-008 | Phase 0 (Phase 0 gate) | EPIC-008 |
| EPIC-010 | Pilot hardening (Tier 3) | Audit of sign-ins and permissions, usage logging and rate limits, backups and restore, rollback, licence ceiling | FR-012, FR-070, FR-075, FR-076, FR-077, FR-100 | US-045 to US-049 | CMP-001, CMP-008, CMP-009, CMP-004 | Phase 1 | EPIC-009 and the Phase 0 gate |
| EPIC-011 | Pilot publishing | Upload, shared sources, Assets, harvested and standard metadata, review queue, approval by tier | FR-013, FR-014, FR-027, FR-034 to FR-037, FR-044, FR-061 | US-050 to US-054 | CMP-004, CMP-006, CMP-001, CMP-002 | Phase 1 (Phase 1 gate) | EPIC-010 |
| EPIC-012 | GIS and BI doors | OGC API Features, REST and OData with caveats; service identities and row-level rules | FR-079, FR-097, FR-098, FR-099 | US-055 to US-058 | CMP-011, CMP-012, CMP-001 | Phase 2 | EPIC-011 and the Phase 1 gate |
| EPIC-013 | Pipeline builder and DAG graph | Owners build pipelines in forms saved to Git, and recover runs themselves | FR-045 to FR-049 | US-059, US-060, US-061 | CMP-013, CMP-020, CMP-002, CMP-006 | Phase 2 | EPIC-011 |
| EPIC-014 | Requests, feedback, console and map preview | Gaps, usefulness and API trials reach owners; spatial products preview on a map | FR-062 to FR-065 | US-062 to US-065 | CMP-001, CMP-002 | Phase 2 | EPIC-012 |
| EPIC-015 | Notifications and alerting | Failed checks reach owners and platform faults reach admins | FR-101 | US-066 | CMP-014, CMP-001, CMP-006 | Phase 2 (Phase 2 gate) | EPIC-011 |
| EPIC-016 | Entra sign-on | One BUUK identity | FR-080 | US-067 | CMP-015, CMP-016, CMP-001 | Phase 3 | EPIC-015 and the Phase 2 gate; DEP-003, DEP-005 |
| EPIC-017 | AI front door (Copilot) | Staff ask in Copilot and get answers with tier and caveats | FR-088 to FR-093 | US-069 to US-072 | CMP-008, CMP-016, CMP-017 | Phase 3 | EPIC-016; DEP-003, DEP-004 |
| EPIC-018 | Governance | Owner succession, access requests, subscriptions, lineage and a usage dashboard | FR-081, FR-102 to FR-105 | US-073 to US-077 | CMP-001, CMP-002, CMP-014, CMP-018 | Phase 3 (Phase 3 gate) | EPIC-016 |
| EPIC-019 | Connector framework, secrets and file formats | Every source is a connector; secrets are held and never logged; more file formats; a source can be tested before its first run | FR-121, FR-122, FR-123, FR-128 | US-082 to US-085 | CMP-004, CMP-021, CMP-001 | Phase 1 | EPIC-011 |
| EPIC-023 | API and database sources | Internal and external HTTP APIs and Postgres or SQL Server databases are sources | FR-015, FR-017, FR-119 | US-080, US-081 | CMP-021, CMP-004 | Phase 1 | EPIC-019 |
| EPIC-021 | Domains, stewardship and environments | Domain stewards and deputies; the owner's get-started guide; separate development, staging and production on the pilot VM | FR-106, FR-116, FR-118 | US-068, US-086, US-087 | CMP-001, CMP-002 | Phase 1 | EPIC-010 |
| EPIC-025 | Door connectors | Every door is a connector behind the access service, with a registry to switch doors off | FR-126, FR-127 | US-090 | CMP-022, CMP-001, CMP-008 | Phase 1 | EPIC-008 and EPIC-010 |
| EPIC-022 | Domain browse and sensitive columns | Browse by domain; sensitive columns kept out of wider audiences | FR-117, FR-124 | US-088, US-089 | CMP-001, CMP-002, CMP-005 | Phase 2 | EPIC-021 and EPIC-025 |
| EPIC-024 | Parked ingest | OGC API ingest, the source-network runner and change-data-capture sources, each when its need is clear | FR-016, FR-120, FR-125 | US-078 | CMP-004, CMP-021 | Phase 4 (parked) | Phase 3 gate or a change request |
| EPIC-020 | Later features | Deferred features, each specified by a change request when picked up | FR-110 to FR-115 | US-079 | CMP-001, CMP-002 | Phase 4 (parked) | A change request per feature |

## Tests
Level is unit, integration, e2e, manual or check. TEST-001 to TEST-079 prove the acceptance criteria of US-001 to US-079 in turn; TEST-101 onwards cover NFR, SEC and PERM items. Database and pipeline tests use throwaway containers and small fixture files, never the full flood map (brief Ground rules), except where marked "real data".

| TEST | Test | Level | Verifies | Where |
|---|---|---|---|---|
| TEST-001 | Stack health, migration idempotence and the one-command test run | integration | US-001-AC1, US-001-AC2, US-001-AC3, FR-001 | `backend/tests/test_migrations.py`, CI |
| TEST-002 | Sample manifest registers the product and audits it | integration | US-002-AC1, US-002-AC2, FR-002, FR-006 | `pipelines/tests/test_manifest_load.py` |
| TEST-003 | Each invalid manifest refused with its message | unit | US-003-AC1, US-003-AC2, US-003-AC3, US-003-AC4, FR-003 | `pipelines/tests/test_manifest_validator.py` |
| TEST-004 | Licence register, licence before ingest, unknown licence cap | integration | US-004-AC1, US-004-AC2, US-004-AC3, FR-004, FR-005 | `backend/tests/test_licences.py` |
| TEST-005 | Known-issue fields, severities and status path | integration | US-005-AC1, US-005-AC2, US-005-AC3, FR-007 | `backend/tests/test_issues.py` |
| TEST-006 | One audit event per action; no rows in audit or usage | integration | US-006-AC1, US-006-AC2, FR-008 | `backend/tests/test_audit.py` |
| TEST-007 | Bronze load from a local path and a fixture HTTP server | integration | US-007-AC1, US-007-AC2, FR-010 | `pipelines/tests/test_ingest.py` |
| TEST-008 | Skip rules on version text and hash | integration | US-008-AC1, US-008-AC2, US-008-AC3, FR-009 | `pipelines/tests/test_ingest.py` |
| TEST-009 | Four-version retention | integration | US-009-AC1, US-009-AC2, FR-011 | `pipelines/tests/test_bronze_retention.py` |
| TEST-010 | Silver build, dbt tests, repair flag and count, CRS check | integration | US-010-AC1, US-010-AC2, US-010-AC3, FR-018 | `dbt/tests`, `pipelines/tests/test_silver.py` |
| TEST-011 | Each boundary check fails on its fixture and passes on the clean one | integration | US-011-AC1, US-011-AC2, FR-021 | `pipelines/tests/test_checks.py` |
| TEST-012 | Issues from blocking and warning failures; last version still served | integration | US-012-AC1, US-012-AC2, US-012-AC3, FR-022 | `pipelines/tests/test_checks.py` |
| TEST-013 | Freshness states with a fixed clock | unit | US-013-AC1, US-013-AC2, US-013-AC3, FR-023, FR-024 | `pipelines/tests/test_freshness.py` |
| TEST-014 | Profile against a hand-checked fixture | integration | US-014-AC1, FR-025 | `pipelines/tests/test_profiling.py` |
| TEST-015 | Drift findings and version bumps | integration | US-015-AC1, US-015-AC2, US-015-AC3, FR-026, FR-107 | `pipelines/tests/test_drift.py` |
| TEST-016 | Gold view flags and metadata tables as the read-only login | integration | US-016-AC1, US-016-AC2, FR-019, FR-020 | `backend/tests/test_sql_door.py` |
| TEST-017 | Score worked example, caps and configurable weights | unit | US-017-AC1, US-017-AC2, US-017-AC3, US-017-AC4, FR-028, FR-029 | `pipelines/tests/test_trust.py` |
| TEST-018 | Override with and without a reason; wrong role refused | integration | US-018-AC1, US-018-AC2, US-018-AC3, FR-030, PERM-006 | `backend/tests/test_tiers.py` |
| TEST-019 | Restore after passing checks; Certified waits for the owner | integration | US-019-AC1, US-019-AC2, FR-031 | `backend/tests/test_tiers.py` |
| TEST-020 | Promotion gate refusals and success | integration | US-020-AC1, US-020-AC2, US-020-AC3, US-020-AC4, FR-032, PERM-004 | `backend/tests/test_promotion.py` |
| TEST-021 | Certified sign-off rules | integration | US-021-AC1, US-021-AC2, US-021-AC3, FR-033 | `backend/tests/test_tiers.py` |
| TEST-022 | DAG factory contract test, schedule change and a fixture run to gold | integration | US-022-AC1, US-022-AC2, US-022-AC3, FR-038, FR-039, FR-040 | `pipelines/tests/test_dag_factory.py`, Airflow test container |
| TEST-023 | Run reporting, run history page and the orchestrator boundary | integration | US-023-AC1, US-023-AC2, US-023-AC3, FR-041, FR-042 | `backend/tests/test_runs.py`, `backend/tests/test_architecture.py`, Playwright |
| TEST-024 | Weekly version check starts or skips a refresh | integration | US-024-AC1, US-024-AC2, FR-043 | `pipelines/tests/test_version_check.py` |
| TEST-025 | Sign-in success, wrong password and deactivated account | integration | US-025-AC1, US-025-AC2, FR-066 | `backend/tests/test_auth.py` |
| TEST-026 | User administration and its permission | integration | US-026-AC1, US-026-AC2, FR-067, PERM-008 | `backend/tests/test_users.py` |
| TEST-027 | Group membership on every door | integration | US-027-AC1, US-027-AC2, FR-068 | `backend/tests/test_groups.py` |
| TEST-028 | Default Named, grant expiry and owner-only changes | integration | US-028-AC1, US-028-AC2, US-028-AC3, FR-069, PERM-005 | `backend/tests/test_access_levels.py` |
| TEST-029 | Access matrix: four doors by Open, Group, Named, owner, steward, admin and outsider; read-only login limits | integration | US-029-AC1, US-029-AC2, US-029-AC3, FR-071, FR-072, FR-073, PERM-002 | `backend/tests/test_access_matrix.py` |
| TEST-030 | Catalogue search, filters and restricted entries | e2e | US-030-AC1, US-030-AC2, FR-050, FR-051, PERM-001 | Playwright |
| TEST-031 | Product page header, tabs, Not set and Access tab | e2e | US-031-AC1, US-031-AC2, US-031-AC3, FR-052 to FR-057 | Playwright |
| TEST-032 | Report an issue | e2e | US-032-AC1, US-032-AC2, FR-058, PERM-007 | Playwright |
| TEST-033 | Steward triage list | e2e | US-033-AC1, FR-059 | Playwright |
| TEST-034 | Owner missing-fields list | e2e | US-034-AC1, FR-060, PERM-014 | Playwright |
| TEST-035 | Personal access tokens | integration | US-035-AC1, US-035-AC2, FR-078, PERM-011 | `backend/tests/test_tokens.py` |
| TEST-036 | Seeded synthetic product published at Group with its issue | integration | US-036-AC1, US-036-AC2, FR-108 | `pipelines/tests/test_synthetic_sites.py` |
| TEST-037 | Spatial join for a member and an outsider; real-data timing recorded | integration | US-037-AC1, US-037-AC2, US-037-AC3, FR-109 | `backend/tests/test_spatial_join.py`; real data: `docs/findings.md` |
| TEST-038 | MCP search and describe | integration | US-038-AC1, US-038-AC2, FR-082, FR-083 | `mcp_server/tests/test_tools.py` |
| TEST-039 | MCP query: combined caveats, row limit, statement guard | integration | US-039-AC1, US-039-AC2, US-039-AC3, FR-084, FR-074 | `mcp_server/tests/test_query.py` |
| TEST-040 | MCP refusals | integration | US-040-AC1, US-040-AC2, US-040-AC3, FR-085 | `mcp_server/tests/test_query.py` |
| TEST-041 | MCP call log; stdio and streamable HTTP with a scripted client | integration | US-041-AC1, US-041-AC2, FR-086, FR-087 | `mcp_server/tests/test_transports.py` |
| TEST-042 | Export bundles, attribution and formats | integration | US-042-AC1, US-042-AC2, US-042-AC3, FR-094, FR-095 | `backend/tests/test_export.py` (GeoPackage read back with GDAL in a test container) |
| TEST-043 | Export refused without access | integration | US-043-AC1, FR-096 | `backend/tests/test_export.py` |
| TEST-044 | The demo script on a clean stack, with the break-and-fix steps; findings note present | manual | US-044-AC1, US-044-AC2, US-044-AC3, FR-109 | `docs/demo-script.md`, recorded run |
| TEST-045 | Audit of sign-ins and permission changes | integration | US-045-AC1, US-045-AC2, FR-077 | `backend/tests/test_audit.py` |
| TEST-046 | Usage events and rate limits per door | integration | US-046-AC1, US-046-AC2, FR-075, FR-076 | `backend/tests/test_usage.py` |
| TEST-047 | Restore drill from the newest dump; backup with the database down | integration | US-047-AC1, US-047-AC2, FR-100 | `scripts/tests/test_backup.sh` |
| TEST-048 | Rollback to a kept bronze version | integration | US-048-AC1, FR-012 | `pipelines/tests/test_rollback.py` |
| TEST-049 | Licence ceiling | integration | US-049-AC1, FR-070 | `backend/tests/test_access_levels.py` |
| TEST-050 | Upload of CSV and GeoPackage; refusals | integration | US-050-AC1, US-050-AC2, FR-013 | `backend/tests/test_upload.py` |
| TEST-051 | Shared source ingested once; Asset-triggered downstream run | integration | US-051-AC1, US-051-AC2, FR-014, FR-044 | `pipelines/tests/test_assets.py` |
| TEST-052 | Metadata harvest and ISO 19139 file validation | integration | US-052-AC1, US-052-AC2, FR-027, FR-061 | `pipelines/tests/test_metadata.py` |
| TEST-053 | Review queue, re-review and reminders with a fixed clock | integration | US-053-AC1, US-053-AC2, US-053-AC3, FR-034, FR-035, FR-036 | `backend/tests/test_review_queue.py` |
| TEST-054 | Approval by tier | integration | US-054-AC1, FR-037 | `backend/tests/test_promotion.py` |
| TEST-055 | OGC API Features items, caveats and access | integration | US-055-AC1, US-055-AC2, FR-097 | `ogc/tests` |
| TEST-056 | REST API response with caveats | integration | US-056-AC1, FR-098 | `backend/tests/test_rest.py` |
| TEST-057 | OData entity set and metadata entity set | integration | US-057-AC1, FR-099 | `backend/tests/test_odata.py` |
| TEST-058 | Service identities and row-level rules | integration | US-058-AC1, US-058-AC2, FR-079 | `backend/tests/test_service_identities.py` |
| TEST-059 | Builder steps, previews and generated SQL | e2e | US-059-AC1, FR-045, FR-046, FR-047 | Playwright |
| TEST-060 | Builder Git save, moved base and edit lock | integration | US-060-AC1, US-060-AC2, FR-048 | `backend/tests/test_builder_git.py` |
| TEST-061 | DAG graph status and retry | e2e | US-061-AC1, FR-049 | Playwright |
| TEST-062 | Data request matching | integration | US-062-AC1, FR-062 | `backend/tests/test_requests.py` |
| TEST-063 | Usefulness ratings grouped by product | integration | US-063-AC1, FR-063 | `backend/tests/test_feedback.py` |
| TEST-064 | API console respects access | e2e | US-064-AC1, FR-064 | Playwright |
| TEST-065 | Map preview with and without access | e2e | US-065-AC1, FR-065 | Playwright |
| TEST-066 | Notification on a failed check | integration | US-066-AC1, FR-101 | `backend/tests/test_notifications.py` |
| TEST-067 | Entra sign-in and group mapping with a test tenant | integration | US-067-AC1, FR-080 | `backend/tests/test_entra.py` |
| TEST-068 | Promotion from staging to production | manual | US-068-AC1, FR-106 | Release checklist |
| TEST-069 | Copilot agent answers with tier and caveats | manual | US-069-AC1, FR-089, FR-092 | Recorded session |
| TEST-070 | Caveat schema and use-case minimum tier | integration | US-070-AC1, US-070-AC2, FR-090, FR-091 | `mcp_server/tests/test_caveats.py` |
| TEST-071 | Evaluation set run report | check | US-071-AC1, FR-093 | `eval/` |
| TEST-072 | Agent report-issue tool | integration | US-072-AC1, FR-088 | `mcp_server/tests/test_tools.py` |
| TEST-073 | Owner succession flag and cap with a fixed clock | integration | US-073-AC1, FR-081 | `backend/tests/test_succession.py` |
| TEST-074 | Access request approve and decline | integration | US-074-AC1, US-074-AC2, FR-103 | `backend/tests/test_access_requests.py` |
| TEST-075 | Subscription notification | integration | US-075-AC1, FR-102 | `backend/tests/test_subscriptions.py` |
| TEST-076 | Lineage impact view | integration | US-076-AC1, FR-104 | `backend/tests/test_lineage.py` |
| TEST-077 | Usage dashboard figures | integration | US-077-AC1, FR-105 | `backend/tests/test_usage.py` |
| TEST-078 | Parked ingest sources | integration | US-078-AC1, FR-016, FR-120, FR-125 | When picked up |
| TEST-079 | Each later feature specified before build | check | US-079-AC1, FR-110 | Change request review |
| TEST-080 | API source: paging, three authentication styles, literal secret refused | integration | US-080-AC1, US-080-AC2, FR-015, FR-119 | `pipelines/tests/test_api_source.py` (fixture API server) |
| TEST-081 | Postgres and SQL Server sources: table, query and incremental load | integration | US-081-AC1, US-081-AC2, FR-017 | `pipelines/tests/test_db_source.py` (fixture containers) |
| TEST-082 | No secret in any log, event, response or file | integration | US-082-AC1, FR-121 | `pipelines/tests/test_secrets.py`, CI |
| TEST-083 | Source test report and its failures | integration | US-083-AC1, US-083-AC2, FR-123 | `backend/tests/test_source_test.py` |
| TEST-084 | JSON Lines, Excel and Parquet upload and content refusal | integration | US-084-AC1, US-084-AC2, FR-122, SEC-013 | `backend/tests/test_upload.py` |
| TEST-085 | Existing ingest unchanged on connectors; a fixture connector lands | integration | US-085-AC1, US-085-AC2, FR-128 | `pipelines/tests/test_connectors.py` |
| TEST-086 | Domain steward scope | integration | US-086-AC1, US-086-AC2, FR-116, PERM-015 | `backend/tests/test_domains.py` |
| TEST-087 | Guide link and example keys validate | integration | US-087-AC1, FR-118 | `pipelines/tests/test_guide_examples.py` |
| TEST-088 | Domain view counts | e2e | US-088-AC1, FR-117 | Playwright |
| TEST-089 | Sensitive column by access level | integration | US-089-AC1, FR-124 | `backend/tests/test_sensitive_columns.py` |
| TEST-090 | Door connectors: existing tests unchanged, registry, fixture door | integration | US-090-AC1, US-090-AC2, US-090-AC3, FR-126, FR-127, PERM-016 | `backend/tests/test_doors.py` |
| TEST-101 | Whole-stack memory during a flood map run | check | NFR-002 | `docker stats` capture in `docs/findings.md` |
| TEST-102 | Spatial join time on the real flood map | check | NFR-003, FR-109 | real data, `docs/findings.md` |
| TEST-103 | Full flood map run time, file size and load time | check | NFR-004, FR-010 | real data, `docs/findings.md` |
| TEST-104 | Catalogue and product page timings with 50 fixture products | integration | NFR-005 | Playwright timing |
| TEST-105 | MCP describe and query timings | integration | NFR-006 | `mcp_server/tests/test_timing.py` |
| TEST-106 | axe on every page in a real browser | e2e | NFR-007 | `frontend/tests/e2e/accessibility.spec.ts` |
| TEST-107 | Audit and usage purge after 12 months with a fixed clock | integration | NFR-009, SEC-007 | `backend/tests/test_retention.py` |
| TEST-108 | DAG factory contract tests pinned to expected DAG structure | integration | NFR-010, FR-038 | `pipelines/tests/test_dag_factory.py` |
| TEST-109 | No model client or AI dependency, and no store other than Postgres, in any image or lock file | check | NFR-011, NFR-012 | CI script |
| TEST-110 | Startup refuses missing or placeholder secrets; gitleaks in CI | check | SEC-002 | CI, `backend/tests/test_settings.py` |
| TEST-111 | Published ports on 127.0.0.1 only; Airflow API server not published | check | SEC-003 | CI compose check |
| TEST-112 | MCP statement guard fuzz cases | unit | SEC-005, FR-084 | `mcp_server/tests/test_guard.py` |
| TEST-113 | Role and action matrix on every API route | integration | PERM-001 to PERM-014, SEC-006 | `backend/tests/test_permissions.py` |
| TEST-114 | AI reachability: only AI-enabled public or synthetic products answer over MCP | integration | SEC-009, FR-085 | `mcp_server/tests/test_query.py` |
| TEST-115 | Pipeline database role cannot touch catalogue or audit | integration | SEC-016 | `pipelines/tests/test_roles.py` |
| TEST-116 | Dependency vulnerability scan in CI | check | SEC-014 | CI (Phase 1) |
| TEST-117 | Upload size and type limits | integration | SEC-013, FR-013 | `backend/tests/test_upload.py` |
| TEST-118 | Password hashing and session cookie flags | integration | SEC-001, SEC-011 | `backend/tests/test_auth.py` |
| TEST-119 | Backup and restore timings within the recovery targets | check | NFR-008 | restore drill record |
| TEST-120 | Token storage and expiry | integration | SEC-008 | `backend/tests/test_tokens.py` |

## Definition of Done
- **Story:** code; the tests named in its acceptance criteria written first and passing in a clean container; the repository's short note updated with what was built and left out (brief Ground rules); every action that the story adds logged to the catalogue or audit log.
- **Epic:** its tests pass in clean containers; the real stack is started and the epic's journey exercised; `check_spec.py --analyze` re-run at its milestone gate.
- **Phase 0 slice:** its Done-when list from the brief passes as tests (EPIC-001 to EPIC-009 map slices 0 to 12).

## Release acceptance
- **Phase 0 (first build), the brief's acceptance:** every slice's Done-when list passes; the demo script runs without manual fixes (TEST-044); the spatial join ran in acceptable time on the real flood map, or the findings note says where Postgres reaches its limit (TEST-102); nothing produced AI-generated content and nothing used storage other than Postgres (TEST-109); the build-versus-adopt spike has reported (outside this build, ASM-011). The spec conformance audit passes before Gate 3.
- **Phase 1:** Tier 3 applied (D-008): EPIC-010, EPIC-011, EPIC-019, EPIC-021, EPIC-023 and EPIC-025 done, the security standard's tier 3 row met; pilot users find, trust and use products without help (OBJ-002); at least one internal API product and one on-premises database product are live, owned by two different domains, with no secret visible anywhere (CR-001).
- **Phase 2:** GIS and BI users connect with their own tools and an owner builds a pipeline unaided (OBJ-006).
- **Phase 3:** staff reach products through Copilot with their own sign-in, and production runs apart from staging (OBJ-007).
- **Phase 4:** no gate; each item is a change request.
- Known limitations are recorded in `composition.json` and doc 12 at each release.
