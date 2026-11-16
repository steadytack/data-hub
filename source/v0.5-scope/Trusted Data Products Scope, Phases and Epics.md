# Trusted Data Products: Scope, Phases and Epics

Nov 13, 2026 · @Tom Duncan

The platform is built in phases, each ending in a gate that has to pass before the next starts. Phase 0 proves the design on one real product. Phase 1 puts a small set of internal products in front of a pilot group. Phase 2 adds more ways to reach and build products. Phase 3 opens the platform to BUUK staff with Entra sign-in and Copilot. Phase 4 is parked until the need is clear. Phases and gates here are proposals to confirm.

## Phases and gates

| Phase | What it delivers | Gate to the next phase |
| --- | --- | --- |
| 0. Prototype | One real product (the Historic Flood Map) and one synthetic product, end to end: pipeline, checks, tier, product page, SQL, file export and one MCP query | The demo script passes, and a spatial join across two products runs in acceptable time on Postgres, and the build-versus-adopt spike has reported |
| 1. Internal pilot | A handful of real internal products, a small pilot group, basic sign-in with roles, and the MCP, SQL and file export doors | Pilot users can find, trust and use products without help |
| 2. Wider access | The OGC API Features, REST and OData doors, the form-based pipeline builder, the read-only DAG graph, data requests, usefulness feedback, the API console, map previews and alerting | GIS and BI users connect with their own tools, and owners build pipelines in the UI without developer help |
| 3. BUUK-wide and governed | Entra single sign-on, separate staging and production environments, the Copilot route, lineage, access requests, usage dashboards and subscriptions | Staff reach products through Copilot with their own sign-in, and the platform runs in separate environments |
| 4. Parked | API polling and OGC API ingest beyond the first sources, database pulls, and the deferred features | No gate planned. Each item is picked up only when the need is clear |

### Build-versus-adopt checkpoint

In parallel with the Phase 0 build, run a short, time-boxed spike that loads the flood map and the synthetic sites product into OpenMetadata and the community edition of Entropy Data, and judges them against the criteria in the consolidated spec. The result is reported at the Phase 0 gate and decides one of three outcomes: keep building everything, build the trust layer on top of an adopted catalogue engine, or adopt one tool and extend it. If the second or third outcome is chosen, epics 6, 7 and 10 are the ones most likely to shrink.

## Phase 1 scope

Phase 1 is the first version people outside the build team use. It is deliberately smaller than the full spec.

### In

- Postgres with PostGIS, with separate bronze, silver and gold schemas and a catalogue schema.
- Pipelines as a manifest and dbt models in Git, with generated DAGs on an embedded Airflow, and a run history page in the app.
- Ingest by file download or upload, with source version detection.
- Profiling, checks at layer boundaries, freshness, drift detection, and known issues opened automatically from failed checks.
- Trust score, tiers, steward override, and promotion by the pipeline owner, with steward sign-off for Certified.
- Licence register and known-issues register, with a report-an-issue button.
- Catalogue search and filters, and the five-tab product page.
- A simple steward triage list and a review list for missing fields.
- Three access doors: MCP, SQL on published views, and file export with a metadata sidecar and version file.
- Basic sign-in with roles, an access level on every product (Open, Group or Named), admin-managed groups, and an audit log.

### Later

- **Phase 2:** OGC API Features, REST, OData, the form-based pipeline builder, the read-only DAG graph, data requests, usefulness feedback, the API console, map previews and alerting.
- **Phase 3:** Entra single sign-on, separate staging and production environments, the Copilot route, lineage, access requests, usage dashboards and subscriptions.
- **Phase 4 (parked):** API polling and OGC API ingest beyond the first sources, database pulls, and the deferred features listed in the spec.

### Out for now

- Any AI-generated content inside the platform.
- Limitations statements, assurance statements, privacy and disclosure controls, a conforms-to field, and style and guidance files.
- An editable graph canvas, and the platform's own chat interface.
- Storage engines beyond Postgres.

## Epics at a glance

Eleven epics cover the whole platform. Most span more than one phase, so each is split by phase in the detail that follows.

| Epic | Outcome | Phases | Depends on |
| --- | --- | --- | --- |
| 1. Foundation and environments | A repeatable stack that anyone can start, test and deploy | 0, 1, 3 | None |
| 2. Identity and access | Every door enforces who can see what | 1, 2, 3 | 1 |
| 3. Catalogue core and registers | Products, versions, owners, manifests, licences and known issues are recorded | 0, 1 | 1 |
| 4. Pipelines and orchestration | Pipelines are defined as code, generated into DAGs, and promoted to gold | 0, 1, 2 | 1, 3 |
| 5. Ingestion | Data from outside arrives in bronze, versioned and traceable | 0, 1, 4 | 4 |
| 6. Quality, checks and trust | Every product carries checks, freshness, issues, a score and a tier | 0, 1 | 3, 4 |
| 7. Product pages and discovery | People can find a product and see why to trust it | 1, 2 | 3, 6 |
| 8. Access doors | Products are reachable through MCP, SQL, file export and later more | 0, 1, 2 | 2, 3, 6 |
| 9. AI front door | Staff reach products through Copilot with caveats intact | 3 | 2, 8 |
| 10. Governance and operations | Alerts, subscriptions, ownership, lineage and monitoring keep it healthy | 2, 3 | 4, 8 |
| 11. Later features | Deferred ideas, picked up when the need is clear | 4 (parked) | Varies |

## Epic detail: foundation, identity, catalogue and pipelines

### Epic 1. Foundation and environments

**Done when** a new developer can start the whole stack and run the tests with one command.

- **Phase 0:** a repository with conventions, and CI that runs tests and linting on every change.
- **Phase 0:** a local stack with Postgres and PostGIS, the app and the embedded Airflow, started together.
- **Phase 0:** database migrations that create the bronze, silver, gold and catalogue schemas.
- **Phase 1:** configuration and secrets kept outside the code, and backup and restore for the catalogue and data databases.
- **Phase 3:** separate development, staging and production environments, with a promotion path between them.

### Epic 2. Identity and access

**Done when** a user only ever sees the products they are allowed to, through every door.

- **Phase 1:** basic sign-in with four roles: consumer, owner, steward and admin.
- **Phase 1:** an access level on every product, Open, Group or Named, starting as Named and enforced on every door, with read-only database roles on published views only.
- **Phase 1:** row limits on queries from tools, admin-managed groups, named grants with an optional expiry, and an audit of sign-ins and permission changes.
- **Phase 3:** Entra single sign-on, with Entra groups mapped to platform groups and roles.
- **Phase 2:** service identities with tokens for MCP and agents, and row-level security where a product needs it.

### Epic 3. Catalogue core and registers

**Done when** a product can be registered with a valid manifest and licence, and issues can be raised and tracked against it.

- **Phase 0:** the product, version and owner model, with authority recorded separately from quality.
- **Phase 0:** a manifest format and validator that refuses publishing when required fields are missing.
- **Phase 0:** the licence register, with attribution wording and whether AI use is allowed.
- **Phase 0:** the known-issues register, with scope, severity, status and a link to the fix at source.
- **Phase 1:** an audit log of every publish, promotion and override.

### Epic 4. Pipelines and orchestration

**Done when** a pipeline written as a manifest and dbt models runs on a schedule and publishes a gold product.

- **Phase 0:** a DAG factory that generates an Airflow DAG from each manifest, on an embedded Airflow that users never open.
- **Phase 0:** dbt models for silver and gold, with each model run as its own task.
- **Phase 0:** a run history in the app showing status, duration and the log for each step.
- **Phase 1:** the promotion gate, where the pipeline owner promotes only when checks pass and the manifest is complete.
- **Phase 2:** the form-based pipeline builder, with a step library, live previews and read-only generated SQL.
- **Phase 2:** a read-only graph of the generated DAG, with status on each task and rerun and retry buttons.

## Epic detail: ingestion, trust, product pages and access

### Epic 5. Ingestion

**Done when** a new external source can be added by writing a manifest, with no new code for a file or a link.

- **Phase 0:** download a file from a link and land it unchanged in bronze, with its hash, load time, link and source version.
- **Phase 0:** skip the run when the source version has not changed.
- **Phase 1:** manual upload of CSV and GeoPackage files, and bronze sources shared across pipelines.
- **Phase 4 (parked):** polling of an API for fast-refresh sources, such as a feed updated every 15 minutes, and ingest from OGC API Features.
- **Phase 4 (parked):** pulls from external databases.

### Epic 6. Quality, checks and trust

**Done when** every published product shows its checks, freshness, open issues, score and tier, and a failed blocking check changes what consumers see.

- **Phase 0:** profiling on arrival and on every refresh: types, ranges, null rates, row counts, geometry type and coordinate system.
- **Phase 0:** checks at each layer boundary, each marked blocking or warning in the manifest.
- **Phase 0:** freshness against the schedule as Fresh, Late or Stale, with created, revised, published and last-run dates kept separately.
- **Phase 0:** schema drift compared against a baseline, and a failed blocking check that opens a known issue and lowers the tier.
- **Phase 1:** a trust score from authority, freshness, checks, open issues and completeness, proposing a tier, with steward override and a recorded reason.
- **Phase 1:** automatic tier restore once checks pass, with owner confirmation for Certified.

### Epic 7. Product pages and discovery

**Done when** someone who has never seen a product can find it, see why to trust it and know how to use it.

- **Phase 1:** catalogue search with filters for domain, owner, tier, access level and spatial or not, where Group and Named products show their name, description, owner and tier but not their data.
- **Phase 1:** the five-tab product page, built from the manifest, profiling and registers, with the tier, access level, freshness and owner in the header.
- **Phase 1:** a report-an-issue button, a steward triage list and a review list for missing fields.
- **Phase 2:** a data request form, a usefulness prompt and an API console.
- **Phase 2:** a map preview for spatial products.

### Epic 8. Access doors

**Done when** the same product, with the same tier and caveats, is reachable through each door.

- **Phase 0:** SQL on published views, with read-only roles scoped to the gold schema and metadata tables for the tier and issues.
- **Phase 0:** file export with a metadata sidecar and a version file, in CSV and, for spatial products, GeoPackage.
- **Phase 0:** an MCP server with search, describe and query tools, returning data with its tier and caveats.
- **Phase 1:** usage logging and rate limits across all doors.
- **Phase 2:** OGC API Features for spatial products, a versioned REST API, and an OData feed with a metadata entity set for Power BI and Excel.

## Epic detail: AI front door, governance and later features

### Epic 9. AI front door

**Done when** a member of staff asks a question in Copilot and gets an answer that carries the tier and caveats of the data behind it.

- **Phase 3:** the MCP server made remote over HTTPS with the streamable transport, on a domain name with a certificate, authenticating through Entra.
- **Phase 3:** caveats structured in MCP responses so an agent can act on them, not only read them.
- **Phase 3:** the minimum tier per use case enforced, and the weakest-input rule applied to answers that draw on several products.
- **Phase 3:** a Copilot agent that wraps the MCP server, built in Copilot Studio or as a declarative agent.
- **Phase 3:** an evaluation set of questions, such as the critical buildings example, that checks answers respect tiers and caveats.
- **Phase 3:** a report-an-issue tool for agents.

This epic depends on two outside confirmations: that BUUK IT and security allow approved AI tools to connect to an external MCP server, and what Copilot licences and costs apply.

### Epic 10. Governance and operations

**Done when** problems reach the right person without anyone watching a dashboard.

- **Phase 2:** notifications to owners when checks fail, and monitoring and alerting for the platform itself.
- **Phase 3:** owner succession, with a named deputy, a flag when an owner's account is disabled, and a quarterly attestation.
- **Phase 2:** an Airflow upgrade policy, with contract tests for the DAG factory.
- **Phase 3:** consumer subscriptions to changes, and an access request workflow for restricted products.
- **Phase 3:** lineage from Airflow and dbt events, and a usage dashboard for admins.

### Epic 11. Later features

Nothing here is scheduled. Each item is picked up when the need is clear.

- AI-drafted metadata, always confirmed by a person.
- Limitations and assurance statements, privacy and disclosure controls, a conforms-to field, style and guidance files with downloads, and Excel and GeoJSON export.
- An editable graph canvas on the same manifest.
- Saved and shareable charts and maps, a Python client, a glossary, a public roadmap dataset, dashboards and a reuse showcase.
- Additional storage for very large datasets, if the Phase 0 tests show Postgres is not enough.

## Build order and open decisions

Epics are built in the order their dependencies allow. The Phase 0 prototype build brief covers steps 1 to 4 for the flood map example.

1. **Foundation first** (epic 1), then the catalogue core and registers (epic 3), because everything else records into them.
2. **The pipeline spine** (epics 4 and 5), with ingestion and the DAG factory built together so one product can run end to end.
3. **Quality and trust** (epic 6), added as checks, freshness and the tier on that same product.
4. **The first doors** (epic 8), starting with SQL, file export and MCP, so the product can be used.
5. **Product pages and sign-in** (epics 7 and 2), which turn the working spine into something a pilot group can open.
6. **Phase 1 gate.** Phase 2 then adds the extra doors, the form builder, the DAG graph and alerting (epics 8, 4, 7 and 10). Phase 3 adds Entra, the Copilot route, separate environments and the governance features (epics 2, 9, 1 and 10). Phase 4 stays parked until the need is clear.

### Decisions

- **Sign-in in Phase 1:** decided. Phase 1 uses basic sign-in with roles, and Entra single sign-on moves to Phase 3 alongside the Copilot route. Access uses three levels, Open, Group and Named, described in the spec's Permissions section.
- **Pilot products:** which handful of real internal products go first, and who owns each.
- **First owners and stewards:** the named people who will promote and sign off products in the pilot.
- **Hosting:** decided. The stack runs in Docker, the server can reach the internet to download public data, and the project has its own new Git repository.
- **Outside confirmations:** left open for now. The IT and security view on MCP, the Microsoft licences and costs, Entra and the network setup (domain and HTTPS) are delayed until after the prototype. MCP is tested with local clients that need none of them.
- **Team and capacity:** who builds each epic, which sets how fast phases can move. No durations are given here for that reason.
