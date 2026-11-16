# 02 Product definition

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-02 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

## Vision
BUUK publishes its data as versioned, documented products that people and AI tools can find, query and rely on, with the trust tier and known issues travelling with every answer: "we know exactly how far to trust it" instead of "we can't trust it for AI".

## Objectives and success measures
| OBJ | Objective | Success measure | Source |
|---|---|---|---|
| OBJ-001 | A product goes from its source to a published, tiered product with no hand-built page or hand-written DAG | The flood map runs from download to gold through the generated DAG; adding another file source needs a manifest only, no new code | brief slice 7; scope Epic 5 |
| OBJ-002 | Consumers can tell how far to trust each product | Every product page shows tier, freshness, owner and open known issues; at the Phase 1 gate, pilot users complete at least 4 of 5 set tasks (find, judge, query, download, report) without help | v0.5 Trust is a gradient; scope Phase 1 gate |
| OBJ-003 | Caveats travel with the data through every door | 100 % of responses through the product page, SQL metadata tables, MCP and export carry tier, version and open issues (tested per door) | v0.5 Access |
| OBJ-004 | A combined answer carries every input's caveats and the weakest tier | The cross-product MCP query returns both products' caveats and the weaker tier, every time | brief slice 10 |
| OBJ-005 | Access is enforced the same way on every door | The per-door access test matrix has 0 failures: a user outside a product's access level reads none of its data through the page, SQL, MCP or export | brief slice 8 |
| OBJ-006 | (Phase 2) GIS and BI users connect with their own tools, and owners build pipelines without developer help | QGIS reads the flood map through OGC API Features and Power BI reads an OData feed; one owner builds and publishes a pipeline in the UI unaided | scope Phase 2 gate |
| OBJ-007 | (Phase 3) Staff reach products through Copilot with their own sign-in, in separate environments | Every question in the evaluation set returns its tier and caveats through Copilot; production runs apart from staging | scope Phase 3 gate |
| OBJ-008 | The prototype runs and demonstrates on one host | The demo script runs start to finish on a clean stack in about 15 minutes with no manual fixes; the stack stays within 8 GB of memory; the spatial join takes under 10 seconds, or the findings note says where Postgres reaches its limit | brief Acceptance for Phase 0; ASM-010 |
| OBJ-009 | Nothing in the platform is written by a model, and everything is stored in Postgres | 0 AI-generated fields and 0 non-Postgres stores at each phase gate | brief Ground rules |

## Users and stakeholders
| Who | How often | Device | Main things they do |
|---|---|---|---|
| Consumer (analyst, GIS user, AI assistant user) | Weekly | Desktop browser, SQL client, QGIS, VS Code or MCP Inspector | Search, read a product page, query by SQL or MCP, download, report an issue |
| Owner (pipeline owner) | Per product, then at each refresh | Desktop browser, Git | Write the manifest and dbt models, fill missing fields, promote silver to gold |
| Steward | Weekly triage | Desktop browser | Triage findings and issues, override tiers with a reason, sign off Certified |
| Admin | Occasionally | Desktop browser | Users, roles, groups, grants, backups |
| The platform (scheduled) | Quarterly refresh, weekly version check | Airflow | Ingest, transform, check, profile, score, report |

Phase 0: the build team and the demo audience. Phase 1: a small named pilot group. Phase 3: BUUK staff. Stakeholders: BUUK IT and security (approval of AI tools reaching an external MCP server), the Microsoft 365 admin (Copilot licences), and the Environment Agency as the publisher of the first product.

## Journeys
| JRN | Journey | Actor | Objective | Source |
|---|---|---|---|---|
| JRN-001 | Publish a product: write the manifest, run the pipeline, review the generated page, fill the gaps, promote to gold | Owner | OBJ-001 | v0.5 Worked example: flood map |
| JRN-002 | Find a product and judge whether to trust it from its page | Consumer | OBJ-002 | brief slice 8 |
| JRN-003 | Query a product's published view with SQL and read its tier and issues from the metadata tables | Consumer | OBJ-003 | brief Demo step 4 |
| JRN-004 | Download a product as a file with its sidecar and version file | Consumer | OBJ-003 | brief slice 11 |
| JRN-005 | Ask an AI assistant connected over MCP: search, describe, query across products, answer with combined caveats | Consumer through an AI tool | OBJ-004 | brief slice 10 |
| JRN-006 | Triage findings and reported issues, override a tier, sign off Certified | Steward | OBJ-002 | v0.5 Known-issues register |
| JRN-007 | Manage users, roles, groups and grants, and set a product's access level | Admin and owner | OBJ-005 | brief slice 8 |
| JRN-008 | Refresh on schedule: check the source version, ingest, build, check, profile, score, and change the tier when a check fails or passes | The platform | OBJ-001 | v0.5 Worked example step 10 |
| JRN-009 | Report an issue found while using a product | Consumer | OBJ-002 | v0.5 A feedback loop |
| JRN-010 | (Phase 2) Connect QGIS, Power BI, Excel or an internal app through OGC API Features, OData or REST | Consumer | OBJ-006 | v0.5 How to connect to a product |
| JRN-011 | (Phase 2) Build a pipeline in the form-based builder and watch its DAG run | Owner | OBJ-006 | v0.5 Pipeline builder UI |
| JRN-012 | (Phase 3) Ask a question in Copilot or Teams and get an answer carrying tier and caveats | BUUK staff member | OBJ-007 | scope Epic 9 |
| JRN-013 | (Phases 2 and 3) Ask for a dataset, for access to a restricted product, or to be told when a product changes | Consumer | OBJ-005 | v0.5 Requests; scope Epic 10 |
| JRN-014 | Run the platform: start it, back it up, roll a bad load back, see usage, promote between environments | Admin | OBJ-008 | scope Epic 1, Epic 10 |

## In scope
All phases are specified in full (D-007); Phase 0 is the first build.
- **Phase 0, prototype (Build Brief 001, slices 0 to 12):** the stack; catalogue, manifest and licence register; ingest to bronze with version detection; silver and gold with dbt; checks, freshness, profiling and drift; trust score, tiers, override, restore and promotion; DAG factory on embedded Airflow with Cosmos; sign-in with four roles, groups and named grants; catalogue list and five-tab product page; report an issue, steward list and missing-fields list; the synthetic sites product and the spatial join; the MCP server; file export; the demo script and findings note.
- **Phase 1, internal pilot (Tier 3, D-008), on a BUUK-hosted VM (DEC-016):** domain stewards, source connectors (API, Postgres and SQL Server, more file formats), door connectors and registry, a secrets store, a source test, separate environments, the get-started guide (CR-001), backups and restore, rollback, upload, shared bronze sources, metadata harvest and ISO 19139 file, review queue, re-review, reminders, approval by tier, Airflow Assets, licence ceiling, rate limits, usage logging, audit of sign-ins and permission changes.
- **Phase 2, wider access:** OGC API Features, REST and OData doors; service identities and row-level security; the form-based pipeline builder and DAG graph; data requests, usefulness feedback, API console and map preview; notifications and alerting.
- **Phase 3, BUUK-wide and governed:** Entra sign-on, separate environments, remote MCP and the Copilot agent, structured caveats, use-case minimum tiers, the evaluation set, the agent report-issue tool, owner succession, access requests, subscriptions, lineage and the usage dashboard.
- **Phase 4, parked (Could):** OGC API ingest, the source-network runner, change-data-capture sources, and the later features (FR-110 to FR-115).

## Out of scope and non-goals
- Any AI-generated content inside the platform (until FR-110, if ever picked up); the platform's own chat interface.
- Any storage other than Postgres; any BUUK internal, asset, customer or critical-site data until the data rules are agreed with IT and security.
- Public access with no sign-in.
- The build-versus-adopt spike (ASM-011): run separately, reported at the Phase 0 gate.
- Choosing the pilot products and the named first owners and stewards (Q-001, Q-002).

## Constraints and dependencies
- Postgres 17 with PostGIS 3.5 only; bronze, silver, gold, catalogue and audit as schemas in one database; gold reached only through published views.
- dbt for transforms; an embedded, app-owned Airflow with DAGs generated from manifests; the Airflow UI never exposed.
- Configuration (thresholds, weights, URLs, schedules) in YAML; secrets from the environment.
- Public (Open Government Licence) and synthetic data only in Phases 0 and 1.
- Phase 3 depends on BUUK IT and security, the Microsoft 365 admin and an Entra tenant (DEP-003, DEP-004, DEP-005).

## Exit or retirement criteria
- At the Phase 0 gate the build-versus-adopt spike decides: keep building everything, build the trust layer on an adopted catalogue engine, or adopt one tool and extend it. The second and third would shrink EPIC-004, EPIC-006 and EPIC-011 (scope: epics 6, 7 and 10 of the source).
- The platform is retired if pilot users cannot find, trust and use products without help at the Phase 1 gate and the gap cannot be closed.
