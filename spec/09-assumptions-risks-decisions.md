# 09 Assumptions, risks and decisions

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-09 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

Status: open, closed, accepted, withdrawn. Blocks: none, spec, build, release (an open item that blocks spec stops Gate 2). Facts are not assumptions; unknowns are written "unknown".

## Assumptions
| ASM | Assumption | If wrong | Status | Owner | Blocks |
|---|---|---|---|---|---|
| ASM-001 | (Withdrawn at Gate 1: the spec covers all phases in full, D-007) | - | withdrawn | owner | none |
| ASM-002 | The front end is the template's React front end | A different stack changes EPIC-006's components | accepted | owner | none |
| ASM-003 | Airflow 3.x with matching Cosmos and dbt-postgres, pinned with the constraints file; versions fixed after an install check | Now fixed: Airflow 3.4.0, Cosmos 1.16.0, dbt-core 1.12.7, dbt-postgres 1.11.0 (DEC-002) | closed | factory | none |
| ASM-004 | MCP identity in Phases 0 to 2 is a personal access token created by the signed-in user; Entra replaces it in Phase 3 | Another identity route changes FR-078 and CMP-008 | accepted | owner | none |
| ASM-005 | Stewards and admins do not see Group or Named data unless granted; owners always see their own; a product's existence is never hidden from signed-in users | RULE-008 and the access matrix change | accepted | owner | none |
| ASM-006 | First trust-score weights and thresholds: authority 25, freshness 20, checks 30, open issues 15, completeness 10; Certified at 85, Usable with caveats at 60; Not for AI from licence or manifest | Only the YAML changes (RULE-005, RULE-006) | accepted | owner | none |
| ASM-007 | Quarterly freshness: Fresh within 100 days of the release month, Late to 130, Stale after | Only the YAML changes (RULE-004) | accepted | owner | none |
| ASM-008 | The MCP caller passes its minimum tier; the configured floor is Exploratory only in Phase 0 | RULE-010 changes before Phase 3's use-case register | accepted | owner | none |
| ASM-009 | The synthetic sites product is about 200 seeded polygons placed to overlap known flood map areas | FR-108 fixture changes | accepted | owner | none |
| ASM-010 | "Acceptable" spatial-join time is under 10 seconds on the real data for one network type | NFR-003 threshold changes; the findings note records the figure either way | accepted | owner | none |
| ASM-011 | The build-versus-adopt spike is outside this idea and reports at the Phase 0 gate | CAP-025 and EPIC-004, EPIC-006, EPIC-011 may shrink | accepted | owner | none |
| ASM-012 | The Environment Agency's download link and version file stay as recorded in the brief | INT-001 configuration changes (links are in `config/sources.yaml`) | open | owner | none |
| ASM-013 | The flood map's authority is "reference copy" (the EA holds the authoritative record; BUUK serves a copy) | The score changes by at most 5 points (RULE-005) | accepted | owner | none |

## Open questions
| Q | Question | Recommended answer | Status | Owner | Blocks |
|---|---|---|---|---|---|
| Q-001 | Which handful of real internal products go into the Phase 1 pilot, and who owns each? | Decide at the Phase 0 gate, once the data rules are agreed with IT and security | open | owner | release |
| Q-002 | Who are the first named owners and stewards for the pilot? | Name them with Q-001 | open | owner | release |
| Q-003 | Who builds each epic, which sets how fast phases move? | Not needed for Phase 0, which this factory builds | open | owner | none |
| Q-004 | For each pilot source (on other VMs or in Azure or AWS): what is the network route from the hub's VM, the read-only account, and who rotates its credential? | IT and security confirm per source before it is chosen; pick sources already reachable first | open | owner | release |

## Risks
| RISK | Risk | Likelihood | Impact | Mitigation | Status | Owner | Blocks |
|---|---|---|---|---|---|---|---|
| RISK-001 | Embedded Airflow is heavy for one host (several containers, memory, upgrade burden) | medium | medium | Local executor, pinned constraints, contract tests for the DAG factory (NFR-010), memory measured (TEST-101) | open | factory | none |
| RISK-002 | Airflow, Cosmos and dbt version compatibility | medium | high | Install check done (A failed, B passed); dbt in its own environment (DEC-004); a DAG run is EPIC-005's first test | open | factory | none |
| RISK-003 | Access enforced differently on one of the doors | medium | high | One access service (FR-071) and a test matrix per door and level (TEST-029, TEST-113) | open | factory | none |
| RISK-004 | MCP responses go to the model provider behind the assistant | medium | high | AI use off by default; on only where the licence permits and the data is public or synthetic (DEC-007, SEC-009) | open | factory | none |
| RISK-005 | The flood map has almost no attributes, so its dictionary is mostly owner-supplied and completeness lowers the tier until filled | high | low | Expected; shown as Not set; harvest from the EA record in Phase 1 (FR-027) | accepted | owner | none |
| RISK-006 | BUUK material sits in the factory repository | low | medium | Private repository (DEF-14); nothing published without a typed confirmation (POL-01) | accepted | owner | none |
| RISK-007 | Geometry repair and the spatial join on 21,578 multipolygons may be slow | medium | medium | Measured in slice 9 (TEST-102); spatial indexes on the published views; the findings note records where Postgres reaches its limit | open | factory | none |
| RISK-008 | dbt v2 is announced as the default for new installations; dbt Core 1.x may slow down in fixes | medium | low | Pinned 1.12.7 with dbt-postgres 1.11.0 and Cosmos 1.16.0; revisit at the six-monthly Airflow upgrade (NFR-010) | open | factory | none |
| RISK-009 | No real samples yet for the EA metadata record, the Phase 2 and 3 clients, the mail relay, Entra, Copilot and lineage, so items reading them can only be partly verified | high | medium | Save a sample before each epic that reads one (EPIC-011, EPIC-012, EPIC-015 to EPIC-018) | open | factory | none |
| RISK-010 | The five imported libraries were approved on a static review and an install check only | low | medium | Their behaviour is proven by EPIC-003, EPIC-005 and EPIC-008's tests in clean containers | open | factory | none |
| RISK-011 | The MCP query tool's statement guard could let a crafted query reach beyond the published views | low | high | Parsed single SELECT, read-only transaction, the caller's read-only login (which cannot see other schemas anyway), statement timeout, fuzz tests (SEC-005, TEST-112) | open | factory | none |
| RISK-012 | A source credential leaks through a manifest, log or error message | low | high | Secrets store with names only (FR-121, SEC-017); a literal secret is refused; a test searches every log and response (TEST-082); read-only accounts | open | factory | none |
| RISK-013 | A pull loads a production source database or API | medium | medium | Read-only accounts, off-peak schedules, replicas where they exist, timeouts and row and time limits per run (SEC-018, FR-017) | open | owner | release |
| RISK-014 | The hub's VM has no network route to a pilot source, so it cannot become a product | medium | medium | Q-004 per source; choose reachable sources first; the source-network runner (FR-120) is held for a source with no route | open | owner | release |

## Dependencies
| DEP | Dependency | Status | Owner | Blocks |
|---|---|---|---|---|
| DEP-001 | The Environment Agency's Historic Flood Map download and version file stay available, key-less | open | owner | none |
| DEP-002 | The owner approves CAT-100 to CAT-104 in the catalogue (POL-10): approved at Gate 2, 2026-11-14 | closed | owner | none |
| DEP-003 | BUUK IT and security agree whether approved AI tools may connect to an external MCP server | open | owner | release |
| DEP-004 | The Microsoft 365 admin confirms the licences and costs for the Copilot agent | open | owner | release |
| DEP-005 | An Entra tenant, a domain and HTTPS for Phase 3 | open | owner | release |
| DEP-006 | The build-versus-adopt spike reports at the Phase 0 gate (ASM-011) | open | owner | release |

## Decisions
| DEC | Decision | Rationale | Rejected alternatives | Date | Decided by |
|---|---|---|---|---|---|
| DEC-001 | Template web-app (CAT-006), adapted | Postgres with PostGIS, multi-user sign-in, FastAPI and React fit the catalogue and doors (Gate 1, D-010) | web-app plus etl-pipeline | 2026-11-14 | owner |
| DEC-002 | Pin apache-airflow 3.4.0 (constraints-3.4.0, Python 3.12), astronomer-cosmos 1.16.0, dbt-core 1.12.7, dbt-postgres 1.11.0 and mcp 2.3.0 | Latest releases on 2026-11-14; installed cleanly as described in DEC-004 (D-009) | Older Airflow 3.3 line; dbt v2 | 2026-11-14 | factory |
| DEC-003 | The official MCP Python SDK, not fastmcp | Gate 1, D-009; the SDK's own server covers stdio and streamable HTTP | fastmcp | 2026-11-14 | owner |
| DEC-004 | dbt runs from its own virtual environment inside the Airflow image; Cosmos calls it through `dbt_executable_path` | dbt-core 1.12.7 needs `pathspec<1.1`, which Airflow's constraints do not allow (install check A failed, B passed) | One environment (impossible); a separate dbt container (more moving parts) | 2026-11-14 | factory |
| DEC-005 | GeoPackage read and written with Python's standard SQLite module; GDAL only in a test container | Two small jobs on a documented format; no large native dependency (PRINCIPLES) | GDAL in the images; geopandas | 2026-11-14 | factory |
| DEC-006 | A steward override replaces the proposed tier but not the caps: an open blocking issue still caps at Exploratory only | Consumers must always see a blocking caveat; the steward resolves the issue to lift it | Override above every cap | 2026-11-14 | factory |
| DEC-007 | Over MCP, any product with AI use ticked is reachable, limited to what the calling user can access on any door; AI use may be switched on only for public or synthetic data in Phases 0 and 1 (manifest `data_origin`), so the Group-level synthetic product is reachable over MCP for its group only (owner, Gate 2 decision 2.B) | The brief's data rule allows public or synthetic data to reach AI tools, and slice 9 needs the Group product in the MCP join; the definition's wording "only Open products" was narrower than the source | Only Open products over MCP (would break slice 9 and 10) | 2026-11-14 | owner |
| DEC-008 | A product whose licence forbids AI use, or whose manifest has AI use off, shows the tier Not for AI; its score and the tier it would otherwise have are still shown on the Quality tab | ASM-006 and the source's tier table | A separate "AI" badge beside the tier | 2026-11-14 | factory |
| DEC-009 | The MCP query tool takes one read-only SELECT over published views, run under the caller's read-only login | Cross-product spatial joins need SQL; the caller's login enforces access a second time | A fixed query language (could not express the join) | 2026-11-14 | factory |
| DEC-010 | Epics split by phase; Phase 0 epics follow the brief's slices (EPIC-001 to EPIC-009) | The source's epics span phases; the brief is the build order | One epic per source epic | 2026-11-14 | factory |
| DEC-011 | Tier 2 for Phase 0, Tier 3 from EPIC-010 | Gate 1, D-008 | Tier 3 throughout | 2026-11-14 | owner |
| DEC-012 | All phases specified in full; Phase 0 built first | Gate 1, D-007 | Phase 0 in full, later phases outlined | 2026-11-14 | owner |
| DEC-013 | Template features: Share off; feedback stored in the app, not emailed | Data must not leave by link; no mail service (D-006, Gate 1) | DEF-26 defaults | 2026-11-14 | owner |
| DEC-014 | An export is one zip holding the data file, the sidecar (`metadata.json`) and the version file (`VERSION.txt`) | The three travel together and cannot be separated by a browser | Three separate downloads | 2026-11-14 | factory |
| DEC-015 | The MCP server's HTTP transport binds to 127.0.0.1 like every other port until Phase 3's reverse proxy (POL-06) | Local clients only before Phase 3 | Exposing it earlier | 2026-11-14 | factory |
| DEC-016 | From Phase 1 the pilot runs on a BUUK-hosted VM or server on the internal network, with separate development, staging and production stacks (FR-106) | Internal sources and on-premises databases must be reachable and the data must stay inside BUUK; the owner's reply, CR-001 | A cloud tenant (reviewed again at Phase 3); the owner's machine | 2026-11-16 | owner |
| DEC-017 | Every source type is a connector with one interface (check version, fetch, describe); bronze and everything after it are unchanged | Sources can expand over time without redesign (FR-128) | One-off ingest code per source | 2026-11-16 | owner |
| DEC-018 | Every door is a connector behind the one access service, with limits and usage logging applied by the platform (FR-126, FR-127) | Doors can expand over time without weakening access (RISK-003) | Separate code per door | 2026-11-16 | owner |
| DEC-019 | The first database source connectors are Postgres and SQL Server; Oracle is added later as a further connector | BUUK runs a mix of Postgres, SQL Server and Oracle; the hub's own store is Postgres (NFR-012) | Oracle first; all three at once | 2026-11-16 | owner |

## Change requests
| CR | Change | Reason | Affected IDs | Status | Date |
|---|---|---|---|---|---|
| CR-001 | Company-wide data library and wider ingest: domain stewards, source and door connectors, API and database sources from Phase 1, more file formats, secrets store, environments on a BUUK VM, get-started guide | The platform is to serve all of BUUK and take data from databases, internal and external APIs and files | FR-015, FR-017, FR-106, FR-116 to FR-128; EPIC-016, EPIC-019 to EPIC-025; CMP-021, CMP-022; INT-019 to INT-022; PERM-015 to PERM-017; SEC-017, SEC-018; Q-004; RISK-012 to RISK-014; DEC-016 to DEC-019 | Approved 2026-11-16 (`../changes.md`) | 2026-11-16 |
