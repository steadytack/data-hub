# 08 User stories

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-08 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

As a [role], I want [capability], so that [value]. The acceptance criteria define done; include negative and edge cases. Verification is the TEST in doc 11 that proves it (TEST-nnn proves US-nnn's criteria), with its intent.

## Stories
| US | Role | Goal | Value | FR | Epic | Priority |
|---|---|---|---|---|---|---|
| US-001 | Admin | Start the whole stack and run the tests, each with one command | A new developer can work on it at once | FR-001 | EPIC-001 | Must |
| US-002 | Owner | Register a product by loading its manifest | No hand-built product records | FR-002, FR-006 | EPIC-002 | Must |
| US-003 | Owner | Be told exactly which manifest fields are wrong | I can fix a manifest without guessing | FR-003 | EPIC-002 | Must |
| US-004 | Steward | Keep one licence register that every product links to | Terms are enforced and attribution is right | FR-004, FR-005 | EPIC-002 | Must |
| US-005 | Steward | Record known issues with scope, severity, measure and status | Caveats are specific enough for an agent to act on | FR-007 | EPIC-002 | Must |
| US-006 | Admin | See an audit event for every change | A challenged answer or decision can be traced | FR-008 | EPIC-002 | Must |
| US-007 | Owner | Have the source file land in bronze unchanged with its hash and version | Every load is traceable | FR-010 | EPIC-003 | Must |
| US-008 | Owner | Have unchanged sources skipped | Nothing is reloaded for no reason | FR-009 | EPIC-003 | Must |
| US-009 | Admin | Keep the last four bronze versions | A bad load can be rolled back | FR-011 | EPIC-003 | Must |
| US-010 | Owner | Get cleaned, typed silver data in the national grid with every repair counted | Downstream models rest on valid data | FR-018 | EPIC-003 | Must |
| US-011 | Owner | Have checks run at each layer boundary | Failures are caught | FR-021 | EPIC-004 | Must |
| US-012 | Consumer | See a known issue open when a check fails, while the last good version keeps being served | I am warned without losing the data | FR-022 | EPIC-004 | Must |
| US-013 | Consumer | See how fresh a product is | I know if it is out of date | FR-023, FR-024 | EPIC-004 | Must |
| US-014 | Consumer | See each column's profile | I know the data's shape before querying | FR-025 | EPIC-004 | Must |
| US-015 | Steward | Be shown drift and schema changes between refreshes | Regressions are not silent | FR-026, FR-107 | EPIC-004 | Must |
| US-016 | Consumer | Query a gold view with quality flags and metadata tables beside it | Caveats survive raw SQL | FR-019, FR-020 | EPIC-004 | Must |
| US-017 | Consumer | See a tier proposed by a rule-based score | Trust is stated the same way for every product | FR-028, FR-029 | EPIC-004 | Must |
| US-018 | Steward | Override a tier with a reason | Judgement the score cannot make is recorded | FR-030 | EPIC-004 | Must |
| US-019 | Owner | Have the tier restore once checks pass again | I do not have to chase it | FR-031 | EPIC-004 | Must |
| US-020 | Owner | Promote my product to gold only when it passes the gate | Only passing products are published | FR-032 | EPIC-004 | Must |
| US-021 | Steward | Sign off Certified products | Certified means a person checked it | FR-033 | EPIC-004 | Must |
| US-022 | Owner | Have the DAG generated from my manifest | Nobody writes DAGs by hand | FR-038, FR-039, FR-040 | EPIC-005 | Must |
| US-023 | Owner | See every run and step in the app's run history | I never open Airflow | FR-041, FR-042 | EPIC-005 | Must |
| US-024 | Owner | Have new source versions picked up weekly | Products stay current without me | FR-043 | EPIC-005 | Must |
| US-025 | Consumer | Sign in with my own named account | What I see follows my role and access | FR-066 | EPIC-006 | Must |
| US-026 | Admin | Create users and assign roles | Only named people use the platform | FR-067 | EPIC-006 | Must |
| US-027 | Admin | Create groups and manage members | Group access is easy to keep right | FR-068 | EPIC-006 | Must |
| US-028 | Owner | Set my product's access level, groups and named grants with expiry | The right people see the data, and grants do not linger | FR-069 | EPIC-006 | Must |
| US-029 | Admin | Have every door enforce the same access rule | No door leaks data | FR-071, FR-072, FR-073 | EPIC-006 | Must |
| US-030 | Consumer | Search and filter the catalogue, including products I cannot read | I know what exists and whom to ask | FR-050, FR-051 | EPIC-006 | Must |
| US-031 | Consumer | Read a five-tab product page with tier, access, freshness and owner on top | I see why to trust it and how to use it | FR-052, FR-053, FR-054, FR-055, FR-056, FR-057 | EPIC-006 | Must |
| US-032 | Consumer | Report an issue from the product page | Flaws get fixed for everyone | FR-058 | EPIC-006 | Must |
| US-033 | Steward | Triage open findings and reported issues in one list | I see exceptions, not everything | FR-059 | EPIC-006 | Must |
| US-034 | Owner | See and fill the missing fields of my products | Completeness and the tier go up | FR-060 | EPIC-006 | Must |
| US-035 | Consumer | Create and revoke a personal access token for MCP | My AI tool acts as me and I can cut it off | FR-078 | EPIC-006 | Must |
| US-036 | Owner | Publish a synthetic sites product through the same pipeline | A second product proves the path is general | FR-108 | EPIC-007 | Must |
| US-037 | Consumer | Join two products spatially | Cross-product questions can be answered | FR-109 | EPIC-007 | Must |
| US-038 | Consumer using an AI tool | Search and describe products over MCP | The assistant knows what it is using | FR-082, FR-083 | EPIC-008 | Must |
| US-039 | Consumer using an AI tool | Query over MCP and get the data with its caveats and the weakest tier, within row limits | The assistant cannot drop the caveats | FR-084, FR-074 | EPIC-008 | Must |
| US-040 | Consumer using an AI tool | Get a clear refusal when a product may not be used | The assistant can explain why | FR-085 | EPIC-008 | Must |
| US-041 | Admin | Have every MCP call logged and the server reachable over stdio and HTTP | Answers can be traced and tested locally | FR-086, FR-087 | EPIC-008 | Must |
| US-042 | Consumer | Download a product with its sidecar and version file | The file keeps its context | FR-094, FR-095 | EPIC-008 | Must |
| US-043 | Owner | Have exports refused to people without access | Files do not leak data | FR-096 | EPIC-008 | Must |
| US-044 | Demo audience | Watch one scripted run show the whole design, including a break and a fix | The prototype is judged on evidence | FR-109, FR-022, FR-031, FR-084 | EPIC-009 | Must |
| US-045 | Admin | See sign-ins and permission changes in the audit log | Access changes are accountable at pilot | FR-077 | EPIC-010 | Must |
| US-046 | Admin | Log usage per door and limit request rates | Runaway queries cannot harm the platform | FR-075, FR-076 | EPIC-010 | Must |
| US-047 | Admin | Back up nightly and restore with one command | Data and catalogue survive a failure | FR-100 | EPIC-010 | Must |
| US-048 | Owner | Roll a product back to a kept bronze version | A bad load is undone quickly | FR-012 | EPIC-010 | Must |
| US-049 | Owner | Be stopped from opening a product wider than its licence allows | Licence terms hold | FR-070 | EPIC-010 | Must |
| US-050 | Owner | Upload a CSV or GeoPackage as a source | Sources without a link can be published | FR-013 | EPIC-011 | Must |
| US-051 | Owner | Share one bronze source across pipelines and refresh downstream products when upstream ones update | Nothing is ingested twice and chains stay current | FR-014, FR-044 | EPIC-011 | Must |
| US-052 | Owner | Have source metadata harvested and a standard metadata file generated | I only fill the gaps | FR-027, FR-061 | EPIC-011 | Must |
| US-053 | Owner | Work a review queue of only missing and flagged items, reopened only when something changes, with reminders | I am not the bottleneck | FR-034, FR-035, FR-036 | EPIC-011 | Must |
| US-054 | Owner | Have the approval needed depend on the tier | Low-risk products publish without waiting | FR-037 | EPIC-011 | Must |
| US-055 | Consumer | Add a spatial product to QGIS through OGC API Features | GIS work uses the same trusted products | FR-097 | EPIC-012 | Should |
| US-056 | Consumer | Call a versioned REST API for non-spatial products | Internal apps get data with caveats | FR-098 | EPIC-012 | Should |
| US-057 | Consumer | Read a product as an OData feed in Power BI or Excel | BI users keep the caveats | FR-099 | EPIC-012 | Should |
| US-058 | Admin | Give agents service identities and add row-level rules | Machines get only what they need | FR-079 | EPIC-012 | Should |
| US-059 | Owner | Build a pipeline from a step library with live previews | I need no developer for a simple pipeline | FR-045, FR-046, FR-047 | EPIC-013 | Should |
| US-060 | Owner | Have the builder save to Git and show conflicts instead of merging | Files stay the one source of truth | FR-048 | EPIC-013 | Should |
| US-061 | Owner | See the DAG graph with status and rerun or retry | I can recover a failed run myself | FR-049 | EPIC-013 | Should |
| US-062 | Consumer | Ask for a dataset that is not in the catalogue | Gaps reach a steward | FR-062 | EPIC-014 | Should |
| US-063 | Consumer | Say whether a product was useful | Owners learn what to improve | FR-063 | EPIC-014 | Should |
| US-064 | Consumer | Try the REST and OGC API calls on the Access tab | I can see a call work before coding it | FR-064 | EPIC-014 | Should |
| US-065 | Consumer | Preview a spatial product on a map | I see the coverage at a glance | FR-065 | EPIC-014 | Should |
| US-066 | Owner | Be notified when a check fails, and have admins alerted when the platform is unhealthy | Problems reach the right person | FR-101 | EPIC-015 | Should |
| US-067 | Consumer | Sign in with my BUUK account | One identity everywhere | FR-080 | EPIC-016 | Should |
| US-068 | Admin | Promote changes from staging to production | Production is not where changes are tried | FR-106 | EPIC-021 | Must |
| US-069 | BUUK staff member | Ask Copilot and get an answer from the platform with tier and caveats | I use the tool I already have | FR-089, FR-092 | EPIC-017 | Should |
| US-070 | Consumer using an AI tool | Get structured caveats and have my use case's minimum tier enforced | The agent acts on caveats, not just reads them | FR-090, FR-091 | EPIC-017 | Should |
| US-071 | Steward | Run an evaluation set against the AI front door | I know answers respect tiers and caveats | FR-093 | EPIC-017 | Should |
| US-072 | Consumer using an AI tool | Have the agent report an issue it finds | Flaws found by AI reach the register | FR-088 | EPIC-017 | Should |
| US-073 | Steward | Have products with an absent owner flagged and capped | Every product keeps an accountable owner | FR-081 | EPIC-018 | Should |
| US-074 | Consumer | Request access to a restricted product | Access is asked for and granted in the platform | FR-103 | EPIC-018 | Should |
| US-075 | Consumer | Subscribe to a product's changes | I hear about updates and regressions | FR-102 | EPIC-018 | Should |
| US-076 | Owner | See which products a bad upstream product affects | Impact is visible before consumers find it | FR-104 | EPIC-018 | Should |
| US-077 | Admin | See usage per product and door | I know what is used | FR-105 | EPIC-018 | Should |
| US-078 | Owner | Ingest from OGC API services, from a source network through a runner, and from streams or change capture | The remaining source types can be products when a need is clear | FR-016, FR-120, FR-125 | EPIC-024 | Could |
| US-079 | Consumer | Have the later features when their need is clear | The platform grows where it is used | FR-110, FR-111, FR-112, FR-113, FR-114, FR-115 | EPIC-020 | Could |
| US-080 | Owner | Pull a product's data from an internal or external HTTP API | Fast-refresh API data can be a product without manual exports | FR-015, FR-119 | EPIC-023 | Must |
| US-081 | Owner | Pull a product's data from a Postgres or SQL Server database | On-premises and cloud databases can be products without manual exports | FR-017 | EPIC-023 | Must |
| US-082 | Owner | Keep connection secrets out of my manifest and logs | A leaked manifest or log never leaks a credential | FR-121 | EPIC-019 | Must |
| US-083 | Owner | Test a source before its first run | I find a wrong address or password before the pipeline fails | FR-123 | EPIC-019 | Must |
| US-084 | Owner | Upload JSON, Excel and Parquet files as well as CSV and GeoPackage | The files teams already hold can be sources | FR-122 | EPIC-019 | Must |
| US-085 | Admin | Add a new source type as a connector without changing the platform | Sources can expand over time | FR-128 | EPIC-019 | Must |
| US-086 | Admin | Name a steward and a deputy for each domain | Each domain's quality decisions have an accountable person | FR-116 | EPIC-021 | Must |
| US-087 | Owner | Follow a get-started guide from the app | I can register a product without help | FR-118 | EPIC-021 | Must |
| US-088 | Consumer | Browse the catalogue by domain | I can see what a team has and how trustworthy it is | FR-117 | EPIC-022 | Should |
| US-089 | Owner | Mark columns as sensitive | Personal or restricted fields do not reach wider audiences | FR-124 | EPIC-022 | Should |
| US-090 | Admin | Add doors as connectors and switch them off per product or globally | Consumer access can expand over time without weakening access rules | FR-126, FR-127 | EPIC-025 | Must |

## Acceptance criteria
| AC | Given | When | Then | Verification |
|---|---|---|---|---|
| US-001-AC1 | A clean machine with Docker | `docker compose up` runs | Postgres with PostGIS, the Airflow metadata database, the Airflow scheduler, DAG processor and API server, the backend, the front end and the MCP server all report healthy | TEST-001: stack health check |
| US-001-AC2 | An empty database | The migration runs twice | The five schemas exist and the second run changes nothing | TEST-001: migration test |
| US-001-AC3 | The repository | The test command runs | Backend, pipelines and front-end tests run in clean containers with one command | TEST-001: CI job |
| US-002-AC1 | The flood map sample manifest and the OGL v3 licence entry | It is loaded | A product exists with its owner, source, licence, schedule, Named access unless set, AI use, data origin, domain, authority "reference copy" with its basis, and version 0.1 unpublished | TEST-002: API read-back |
| US-002-AC2 | A manifest has been loaded | The audit log is read | One event records the load, the owner and the time | TEST-002 |
| US-003-AC1 | Manifests each missing one required field | Each is loaded | Each is refused with "[field name]: required" and nothing is registered | TEST-003: one fixture per field |
| US-003-AC2 | A manifest with `access_level: Public` | It is loaded | Refused with "access_level: must be Open, Group or Named" | TEST-003 |
| US-003-AC3 | A manifest with `ai_use: true` and `data_origin: internal` | It is loaded | Refused: AI use is allowed only for public or synthetic data (RULE-001) | TEST-003 |
| US-003-AC4 | A check without a severity | The manifest is loaded | Refused with "checks[0].severity: must be blocking or warning" | TEST-003 |
| US-004-AC1 | No licences | The steward adds OGL v3 with the Environment Agency's attribution wording | The entry holds terms, attribution, permitted uses including AI use, ceiling, manager and dates | TEST-004 |
| US-004-AC2 | A manifest naming a licence that is not in the register | A load is attempted | Refused: "licence before ingest"; no bronze data | TEST-004 |
| US-004-AC3 | A product whose licence entry is marked unknown | Its tier is computed | It is Exploratory only at most | TEST-004 |
| US-005-AC1 | A product | A steward creates an issue with every field | It is stored and listed with status open | TEST-005 |
| US-005-AC2 | An issue | Its severity is set to "critical" | Refused: severity must be minor, material or blocking | TEST-005 |
| US-005-AC3 | An open issue | It is moved through triaged, being fixed and resolved | Each change is stored with who and when | TEST-005 |
| US-006-AC1 | Each auditable action in FR-008 | It happens | Exactly one audit event records actor, action, product and time | TEST-006 |
| US-006-AC2 | An MCP query returning 40 rows | The audit and usage events are read | They hold the row count 40 and no row values | TEST-006 |
| US-007-AC1 | The fixture GeoPackage at a local path | The ingest task runs | Bronze holds the file's features unchanged, with hash, size, load time, link and version text | TEST-007 |
| US-007-AC2 | A fixture HTTP server serving the zip and version file | The ingest runs with the link | The same result as from the local path; no outside network used in tests | TEST-007 |
| US-008-AC1 | A first run has loaded version text "This is the September 2026 release." | A second run sees the same text | A skipped run is recorded and nothing is downloaded | TEST-008 |
| US-008-AC2 | A changed file and version text | The ingest runs | A new bronze version is created | TEST-008 |
| US-008-AC3 | Changed version text but an identical file | The ingest runs | A skip is recorded with reason "same file" | TEST-008 |
| US-009-AC1 | Four kept versions | A fifth load succeeds | The oldest version and its table are removed; four remain | TEST-009 |
| US-009-AC2 | Three kept versions | A run is skipped | Nothing is removed | TEST-009 |
| US-010-AC1 | Bronze holds the fixture | The silver model builds | Columns are standardised and typed, a standard key is added, and dbt tests for key, required columns and geometry validity pass | TEST-010 |
| US-010-AC2 | A fixture with a known self-intersecting polygon | Silver builds | That row is repaired, its repaired flag is true, and the run records a repaired count of 1 | TEST-010 |
| US-010-AC3 | A fixture in EPSG:4326 | Silver builds | The coordinate system check fails as blocking and a known issue opens | TEST-010 |
| US-011-AC1 | Fixtures failing row count, geometry validity, required columns and schema drift in turn | Checks run at each boundary | Each records a failed result for the right rule and boundary | TEST-011 |
| US-011-AC2 | A passing fixture | Checks run | Every result is passed and no issue opens | TEST-011 |
| US-012-AC1 | A blocking check fails | The run finishes | A known issue opens with rule, measured value and scope, severity blocking, and the tier drops | TEST-012 |
| US-012-AC2 | A warning check fails | The run finishes | An issue opens for steward review and the tier is unchanged | TEST-012 |
| US-012-AC3 | A published version and a failing new run | A consumer queries | The last published version still answers | TEST-012 |
| US-013-AC1 | Version text "This is the September 2026 release." and today 2026-11-14 | Freshness is computed | Fresh (74 days), with created, revised, published and last-run dates stored separately | TEST-013 |
| US-013-AC2 | A fixture 131 days past its as-of date | Freshness is computed | Stale, a blocking issue opens and the tier drops | TEST-013 |
| US-013-AC3 | Version text that cannot be parsed and no published date | Freshness is computed | Unknown, and a warning issue opens | TEST-013 |
| US-014-AC1 | A hand-checked fixture of 10 rows | Profiling runs | Types, minimum, maximum, null rate and distinct count per column, row count, geometry type MultiPolygon and EPSG:27700 match the hand-checked values | TEST-014 |
| US-015-AC1 | A refresh where a column's null rate moves from 2 % to 9 % | Profiling compares with the baseline | A drift finding is raised for the steward | TEST-015 |
| US-015-AC2 | A refresh with a dropped column | It is published | The version goes from 1.3 to 2.0 and the owner sees the schema change | TEST-015 |
| US-015-AC3 | An unchanged refresh | It is published | The minor version goes up by 1 and no finding is raised | TEST-015 |
| US-016-AC1 | A published product | The read-only login queries its gold view | Rows include the quality flag columns and flagged rows can be filtered | TEST-016 |
| US-016-AC2 | A published product | The read-only login queries the metadata tables | Tier, freshness, version, attribution and open issues match the catalogue | TEST-016 |
| US-017-AC1 | The flood map example inputs of RULE-005 | The score is computed | It is 91, and the tier is Usable with caveats until a steward signs off | TEST-017 |
| US-017-AC2 | An open blocking issue | The tier is computed | Exploratory only at most | TEST-017 |
| US-017-AC3 | `ai_use: false` or a licence forbidding AI use | The tier is computed | Not for AI, with the score still shown on the Quality tab | TEST-017 |
| US-017-AC4 | The checks weight changed in YAML from 30 to 20 | The score is recomputed | It changes accordingly, with no code change | TEST-017 |
| US-018-AC1 | A steward | They override a tier with a reason | The new tier shows, and the reason and steward are audited | TEST-018 |
| US-018-AC2 | A steward | They override without a reason | Refused | TEST-018 |
| US-018-AC3 | A consumer or owner | They try to override | Refused (PERM-006) | TEST-018 |
| US-019-AC1 | A tier lowered by a failed blocking check | The check passes on the next run | The tier returns to the proposed tier | TEST-019 |
| US-019-AC2 | A Certified product lowered by a failed check | The check passes again | It stays Usable with caveats until the owner confirms, then returns to Certified | TEST-019 |
| US-020-AC1 | A failing silver-to-gold check | The owner promotes | Refused, listing the failing check | TEST-020 |
| US-020-AC2 | Two columns without definitions | The owner promotes | Refused, naming both columns | TEST-020 |
| US-020-AC3 | A product passing RULE-009 | Its owner promotes | The gold view is published as version 1.0 and the promotion is audited | TEST-020 |
| US-020-AC4 | A user who is not the product's owner | They promote | Refused (PERM-004) | TEST-020 |
| US-021-AC1 | A score of 91 with no sign-off | The tier is computed | Usable with caveats | TEST-021 |
| US-021-AC2 | A steward signs off | The tier is computed | Certified, and the sign-off is audited | TEST-021 |
| US-021-AC3 | A steward who owns the product | They sign it off | Refused: an owner cannot sign off their own product | TEST-021 |
| US-022-AC1 | The flood map manifest | The DAG factory runs | One DAG with ingest, one task per dbt model, checks, profiling, gold and promotion-ready tasks, matching the contract test's expected structure | TEST-022 |
| US-022-AC2 | The manifest's cron changed from quarterly to monthly | The DAG folder is parsed again | The DAG's schedule is monthly | TEST-022 |
| US-022-AC3 | The fixture flood map | The generated DAG runs end to end | Gold is reached with no hand-written DAG | TEST-022 |
| US-023-AC1 | A DAG run | Each task ends | Its status, duration and log are in the catalogue's run history | TEST-023 |
| US-023-AC2 | A run with a failed step | The run history page is opened | Every step shows, and the failure links to its step and log | TEST-023 |
| US-023-AC3 | The backend's code | The architecture test runs | Nothing outside the orchestrator interface imports Airflow or calls its API | TEST-023 |
| US-024-AC1 | Changed version text at the weekly check | The check runs | A refresh run starts | TEST-024 |
| US-024-AC2 | Unchanged version text | The weekly check runs | A skip is recorded and no refresh starts | TEST-024 |
| US-025-AC1 | An active account | Its owner signs in with the right password | They are signed in with their role | TEST-025 |
| US-025-AC2 | A wrong password or a deactivated account | Sign-in is attempted | Refused with one generic message | TEST-025 |
| US-026-AC1 | An admin | They create a user and give them the steward role | The user can sign in as a steward | TEST-026 |
| US-026-AC2 | A non-admin | They call the user API | Refused with 403 (PERM-008) | TEST-026 |
| US-027-AC1 | An admin | They create a group and add a member | The member gains access to Group products for that group on every door | TEST-027 |
| US-027-AC2 | A member removed from a group | They query a Group product | Refused on every door | TEST-027 |
| US-028-AC1 | A new product | No access level is set | It is Named and only the owner can read its data | TEST-028 |
| US-028-AC2 | A named grant with expiry 2026-11-14 | On 2026-11-15 the grantee queries | Refused on every door | TEST-028 |
| US-028-AC3 | A user who does not own the product | They change its access level | Refused (PERM-005) | TEST-028 |
| US-029-AC1 | A user outside a Group product's group | They try the page preview, SQL, MCP and export | All four refuse or return no data | TEST-029: access matrix |
| US-029-AC2 | A steward or admin with no grant | They query a Named product | Refused (RULE-008) | TEST-029 |
| US-029-AC3 | A read-only database login | It queries bronze, silver, catalogue and audit | Permission denied on each | TEST-029 |
| US-030-AC1 | Both products published | A consumer searches "flood" and filters by tier, owner, access level, domain and spatial | The results match each filter | TEST-030 |
| US-030-AC2 | A consumer outside the sites group | They search | The sites product shows name, description, owner and tier, with no preview or data | TEST-030 |
| US-031-AC1 | The published flood map | A consumer opens its page | The header shows tier, access level, freshness and owner, and the five tabs match the stored data | TEST-031 |
| US-031-AC2 | Columns without definitions | The Data dictionary tab is opened | They show Not set | TEST-031 |
| US-031-AC3 | A consumer with access | The Access tab is opened | It shows a 50-row preview, downloads, copyable SQL connection details and an MCP snippet | TEST-031 |
| US-032-AC1 | A signed-in consumer | They report an issue with a description | It enters the register with origin user and status open, and shows on the Known issues tab | TEST-032 |
| US-032-AC2 | An empty description | They submit | Refused with "description: required" | TEST-032 |
| US-033-AC1 | Open findings and reported issues | A steward opens the list and triages one | Status, severity, owner and fix link save and are audited | TEST-033 |
| US-034-AC1 | A product with missing definitions | Its owner fills one in the list | It leaves the list and completeness rises | TEST-034 |
| US-035-AC1 | A signed-in user | They create a token | It is shown once, stored hashed, and has an expiry | TEST-035 |
| US-035-AC2 | A revoked or expired token | The MCP server receives it | Refused | TEST-035 |
| US-036-AC1 | The seeded generator | It runs twice with the same seed | The same 200 polygons with network types result | TEST-036 |
| US-036-AC2 | The synthetic manifest | The pipeline runs | The product is published at Group access with its documented known issue open | TEST-036 |
| US-037-AC1 | Fixture flood areas and fixture sites | A group member runs the join for one network type | The expected rows return | TEST-037 |
| US-037-AC2 | A user outside the group | They run the join | Refused | TEST-037 |
| US-037-AC3 | The real flood map | The join runs | Its time and row counts are recorded in the findings note | TEST-037 |
| US-038-AC1 | Both products published | A scripted MCP client calls `search_products` | Each result has name, description, owner, tier, access level and AI use | TEST-038 |
| US-038-AC2 | The flood map | `describe_product` is called | It returns columns with types, units and definitions, EPSG:27700, MultiPolygon, tier, freshness, open issues, licence and version | TEST-038 |
| US-039-AC1 | A user with access to both products | `query_products` runs the cross-product join | Rows return with each product's caveats, the combined caveats and the weaker tier | TEST-039 |
| US-039-AC2 | A query that would return 21,578 rows | It runs | 1,000 rows return with `truncated: true` | TEST-039 |
| US-039-AC3 | A statement that is not one SELECT, or that names a table outside the published views | It is sent | Refused before it runs | TEST-039 |
| US-040-AC1 | A Not for AI product | It is queried | Refused with "not for AI use" | TEST-040 |
| US-040-AC2 | `min_tier: Certified` and a Usable product | It is queried | Refused with "below the minimum tier Certified" | TEST-040 |
| US-040-AC3 | A user without access | They query | Refused with "no access" | TEST-040 |
| US-041-AC1 | Any MCP call | It ends | The log holds user, tool, products, row count and any refusal reason, and no rows | TEST-041 |
| US-041-AC2 | The server | A scripted client connects over stdio and then streamable HTTP | All three tools work over both | TEST-041 |
| US-042-AC1 | A user with access | They download the flood map as GeoPackage | One file holds the GeoPackage, a sidecar with tier, issues, version and licence attribution, and a data version file | TEST-042 |
| US-042-AC2 | The licence register's attribution wording | The sidecar is read | The wording matches exactly | TEST-042 |
| US-042-AC3 | A non-spatial product | GeoPackage is asked for | Refused; CSV is offered | TEST-042 |
| US-043-AC1 | A user without access | They ask for an export | Refused with 403, and the attempt is logged | TEST-043 |
| US-044-AC1 | A clean stack | The demo script's eight steps are followed | Each step shows its expected result in about 15 minutes, with no manual fixes | TEST-044 |
| US-044-AC2 | A published product | A file with a changed schema is loaded, then fixed | A blocking check fails, an issue opens, the tier drops, MCP carries the new caveat; after the fix the tier restores and the owner promotes again | TEST-044 |
| US-044-AC3 | The demo has run | The repository is read | A findings note records load time, file size, join timing, Postgres limits, what was left out and what changed from the spec | TEST-044 |
| US-045-AC1 | Successful and failed sign-ins | The audit log is read | Each is there with user, time and result | TEST-045 |
| US-045-AC2 | Changes to roles, groups, members, access levels and grants | The audit log is read | Each is there with who, what and when | TEST-045 |
| US-046-AC1 | A pull through each door | The usage log is read | One event per pull with user, product, door, time and row count | TEST-046 |
| US-046-AC2 | A user over the rate limit on a door | They send another request | Refused with a retry time | TEST-046 |
| US-047-AC1 | The nightly backup has run | The restore command runs on an empty database | The newest dump restores the data and catalogue | TEST-047 |
| US-047-AC2 | The database is down | The backup runs | It fails loudly and leaves nothing that counts as a fresh backup | TEST-047 |
| US-048-AC1 | A bad load and an earlier kept version | The owner rolls back | Silver and gold are rebuilt from the earlier version and the rollback is audited | TEST-048 |
| US-049-AC1 | A licence whose ceiling is Group | The owner sets Open | Refused, naming the licence ceiling | TEST-049 |
| US-050-AC1 | A CSV and a GeoPackage fixture | The owner uploads them | Each lands in bronze with hash, size and time | TEST-050 |
| US-050-AC2 | A file over the size limit or of another type | It is uploaded | Refused with the reason | TEST-050 |
| US-051-AC1 | Two manifests naming one source | Both pipelines run | The source is ingested once | TEST-051 |
| US-051-AC2 | A downstream product naming an upstream one | The upstream product is refreshed | The downstream DAG runs | TEST-051 |
| US-052-AC1 | A fixture ISO 19139 record linked from a manifest | Harvest runs | Abstract, extent, contact, update frequency and licence fill in; missing ones show Not set | TEST-052 |
| US-052-AC2 | A published version | Its metadata file is generated | It validates against the ISO 19139 schema | TEST-052 |
| US-053-AC1 | Missing fields and flagged items | The owner opens the review queue | Only those show, each with evidence; accept, edit, reject and bulk approve work | TEST-053 |
| US-053-AC2 | A schema change to one column | The refresh publishes | Only that column's review fields reopen | TEST-053 |
| US-053-AC3 | A missing field that will lower the tier in 14 days | The owner signs in | A reminder shows | TEST-053 |
| US-054-AC1 | An Exploratory product and a Usable with caveats product | Each is published | The first publishes without review; the second waits for the owner's confirmation | TEST-054 |
| US-055-AC1 | The flood map | An OGC API Features client asks for items in a bounding box | Features return with paging, and the collection metadata carries tier and caveats | TEST-055 |
| US-055-AC2 | A user without access | They ask for items | Refused | TEST-055 |
| US-056-AC1 | A non-spatial product | The REST API is called with a token | Data returns with tier and caveats under a versioned path | TEST-056 |
| US-057-AC1 | A product | An OData client reads its entity set and metadata entity set | Rows return with select, filter, top and paging, and the metadata set holds tier, freshness, issues, version and licence | TEST-057 |
| US-058-AC1 | A service identity granted one product | It queries another | Refused | TEST-058 |
| US-058-AC2 | A product with a row-level rule | A user covered by the rule queries | Only permitted rows return | TEST-058 |
| US-059-AC1 | The step library | The owner chains Source, Clean, Checks and Publish steps | Each shows a preview and row count, and the generated SQL is read-only until editing is asked for | TEST-059 |
| US-060-AC1 | A pipeline whose base commit has moved | The owner saves | A diff shows for reconciling; nothing is merged automatically | TEST-060 |
| US-060-AC2 | A pipeline open by one owner | A second owner opens it to edit | Refused while the lock is held | TEST-060 |
| US-061-AC1 | A failed run | The owner opens the DAG graph and retries the failed task | The graph shows live status and the retry runs | TEST-061 |
| US-062-AC1 | A request matching an existing product | It is entered | The product is offered before a request is logged | TEST-062 |
| US-063-AC1 | Ratings on a product | The owner opens the feedback view | Ratings and comments are grouped by product | TEST-063 |
| US-064-AC1 | A user without access | They use the console on that product | Refused, as on the API itself | TEST-064 |
| US-065-AC1 | A user with access to the flood map | They open the map preview | The polygons render; without access no preview shows | TEST-065 |
| US-066-AC1 | A failing blocking check | The run ends | The owner receives one notification | TEST-066 |
| US-067-AC1 | A test Entra tenant | A member signs in | They are signed in with the role and groups their Entra groups map to | TEST-067 |
| US-068-AC1 | A manifest change in staging | It is promoted | Production runs the change; staging and production share no database | TEST-068 |
| US-069-AC1 | The remote MCP server behind HTTPS with Entra | A Copilot agent asks a test question | The answer carries the tier and caveats | TEST-069 |
| US-070-AC1 | Any MCP query response | It is validated | It conforms to the published caveat schema | TEST-070 |
| US-070-AC2 | A use case requiring Certified | A Usable product is queried under it | Refused | TEST-070 |
| US-071-AC1 | The evaluation set | It runs | A report lists each question, whether the tier and caveats were respected, and the pass rate | TEST-071 |
| US-072-AC1 | An agent | It calls `report_issue` | An entry with origin AI tool is in the register | TEST-072 |
| US-073-AC1 | A product whose owner's account is disabled | 31 days pass with no reassignment | The product is flagged, then capped below Certified | TEST-073 |
| US-074-AC1 | An access request | The owner approves with an expiry | A named grant is created and the requester can read the data | TEST-074 |
| US-074-AC2 | An access request | The owner declines with a reason | No grant is created and the requester sees the reason | TEST-074 |
| US-075-AC1 | A subscriber | The product is updated | They are notified once | TEST-075 |
| US-076-AC1 | An upstream product feeding two others | Lineage is viewed | Both downstream products show | TEST-076 |
| US-077-AC1 | A week of usage events | The dashboard is opened | Its counts per product and door match the usage log | TEST-077 |
| US-078-AC1 | A fixture OGC API service, an unreachable source with a runner, or a change feed | The need is confirmed and it is picked up | Each lands in bronze with its version, or is specified by a change request first | TEST-078 |
| US-079-AC1 | A later feature picked up | It is specified by a change request | It has its own stories and tests before it is built | TEST-079 |
| US-080-AC1 | A fixture API with three pages | The owner loads its manifest and runs it | All pages land in bronze unchanged with the version and record count | TEST-080 |
| US-080-AC2 | Fixture APIs using an API key, a bearer token and OAuth2 client credentials, and one using cursor paging | Each is run | Each lands in bronze; a manifest with a literal secret is refused | TEST-080 |
| US-081-AC1 | Fixture Postgres and SQL Server containers with a read-only account | Each is run by table and by query | Each lands in bronze as a versioned load | TEST-081 |
| US-081-AC2 | A database source with an incremental key and one new row | The next run happens | Only the new row is added | TEST-081 |
| US-082-AC1 | A run with a known test secret | The logs, audit events, run history and API responses are searched | The secret is in none of them | TEST-082 |
| US-083-AC1 | A fixture API and a fixture database | The owner tests each source | The report shows reachable, authenticated, 10 rows and the schema, and stores no rows | TEST-083 |
| US-083-AC2 | A wrong password | The owner tests the source | The report says "authentication failed" and shows no secret | TEST-083 |
| US-084-AC1 | One fixture each of JSON Lines, Excel and Parquet | Each is uploaded | Each lands in bronze with its row count | TEST-084 |
| US-084-AC2 | A text file named as Excel | It is uploaded | It is refused by content check | TEST-084 |
| US-085-AC1 | The refactored download and upload sources | The existing ingest tests run | They pass unchanged | TEST-085 |
| US-085-AC2 | A fixture connector added without any platform change | It is run | It lands in bronze | TEST-085 |
| US-086-AC1 | An admin and a product in a domain | The admin names a steward and a deputy | The steward can triage and sign off that domain's products | TEST-086 |
| US-086-AC2 | A steward of domain A | They try to override a domain B product | They are refused | TEST-086 |
| US-087-AC1 | An owner on the owner list | They follow the guide link | The guide opens and each key in its examples passes the manifest validator | TEST-087 |
| US-088-AC1 | Fixture products across three domains | The domain view is opened | Counts, tier mix and stale products match the data | TEST-088 |
| US-089-AC1 | A column marked sensitive at Group level and at Named level | Gold and the preview are read | It is absent at Group and present at Named | TEST-089 |
| US-090-AC1 | The refactored doors | The access, MCP, export and permission tests run | They pass unchanged | TEST-090 |
| US-090-AC2 | A door switched off for a product, and a Not for AI product | Each is requested through the doors | The switched-off door and the MCP door refuse them | TEST-090 |
| US-090-AC3 | A fixture door added without any platform change | It is requested | It is served through the access service with limits and logging | TEST-090 |
