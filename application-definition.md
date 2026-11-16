```json definition
{
  "schema_version": 1,
  "slug": "data-hub",
  "name": "Data Hub (Trusted Data Products)",
  "version": "1.0",
  "status": "approved",
  "maturity_tier": {
    "value": 2,
    "state": "accepted",
    "reason": "Owner chose Tier 2 for the Phase 0 prototype, raised to Tier 3 for the Phase 1 pilot (Gate 1, D-008)"
  },
  "deployment": {
    "value": "local",
    "state": "accepted",
    "default": "DEF-02",
    "reason": "The sources decide Docker on one host (app, Postgres/PostGIS, embedded Airflow); local Docker Compose, ports bound to 127.0.0.1 (POL-06)",
    "development": "Same Compose stack on the owner's Windows machine; a BUUK server later is a Publish-time decision"
  },
  "interfaces": {
    "value": [
      "web",
      "non-visual"
    ],
    "state": "accepted",
    "reason": "Web: sign-in, catalogue and the five-tab product page. Non-visual: SQL on published views, file export, the MCP server and the generated Airflow pipelines"
  },
  "brand": {
    "value": "unbranded",
    "state": "accepted",
    "default": "DEF-09",
    "reason": "A prototype; BUUK branding can be added before the pilot"
  },
  "design_system": {
    "value": "The web-app template's design file",
    "state": "accepted",
    "default": "DEF-09"
  },
  "interaction_style": {
    "value": "Search list with filters, tabbed detail page, tables, forms for admin and steward lists",
    "state": "accepted",
    "reason": "The source fixes the catalogue list and the five tabs; no map until Phase 2"
  },
  "template": {
    "value": "web-app",
    "state": "accepted",
    "reason": "Postgres/PostGIS, multi-user sign-in with roles, FastAPI backend and React front end fit the catalogue, product page and API; runner-up web-app plus etl-pipeline, whose own runner and SQLite store the sources replace with Airflow and dbt",
    "options": {
      "share": false,
      "theme": true,
      "help.guides": true,
      "help.feedback": true,
      "feedback.problem": "store",
      "feedback.suggestion": "store",
      "feedback.question": "store",
      "heartbeat": true
    }
  },
  "lenses": {
    "active": [
      "business-value",
      "users-and-workflows",
      "data-and-integrations",
      "architecture",
      "security",
      "ux-and-accessibility",
      "operations",
      "cost-and-maintainability"
    ],
    "skipped": []
  },
  "candidate_assets": [
    {
      "id": "CAT-006",
      "fit": "web-app template: FastAPI, async SQLAlchemy, PostGIS 17-3.5, multi-user JWT sign-in, React front end, Compose stack and CI",
      "limitations": "Sample sites/surveys domain to remove; roles need extending to consumer, owner, steward, admin plus groups and named grants"
    },
    {
      "id": "CAT-054",
      "fit": "dbt-duckdb: shows dbt project conventions only",
      "limitations": "Not a fit: the sources require dbt on Postgres (dbt-postgres, not in the catalogue)"
    },
    {
      "id": "CAT-059",
      "fit": "dagster-postgres: none",
      "limitations": "Not a fit: the source reverses the Dagster decision in favour of Airflow"
    }
  ],
  "sources": [
    {
      "kind": "idea",
      "ref": "idea.md"
    },
    {
      "kind": "document",
      "ref": "source/v0.5/Trusted Data Products Consolidated Spec (v0.5).md",
      "version": "v0.5"
    },
    {
      "kind": "document",
      "ref": "source/v0.5-scope/Trusted Data Products Scope, Phases and Epics.md",
      "version": "v0.5-scope"
    },
    {
      "kind": "document",
      "ref": "source/v0.5-brief-001/Prototype Build Brief 001 Historic Flood Map.md",
      "version": "v0.5-brief-001"
    }
  ],
  "approvals": [
    {
      "gate": 1,
      "date": "2026-11-14",
      "reply": "1. C, 2. B, 3. A (all else as recommended)",
      "version": "1.0"
    }
  ]
}
```

# Data Hub (Trusted Data Products): Application Definition

The block above is the machine-readable record (checked against `factory/schemas/application-definition.schema.json`). Each choice is `recommended`, `accepted`, `rejected`, `deferred` or `unresolved`; `default` names the DEF-nn it came from.

**Organisation.** This idea belongs to BUUK, not to personal work. The owner allowed the three BUUK documents to be imported and committed to the factory repository (2026-11-14, D-001). Only public (Open Government Licence) and synthetic data is used; no BUUK asset, customer or critical-site data enters the factory or the app.

## Source summary
Three documents, imported unchanged into `source/` (counts checked: 47 headings and 14 tables; 24 and 2; 24 and 0).

**Already decided by the sources** (not reopened here):
- Postgres with PostGIS only; `bronze`, `silver`, `gold`, `catalogue` and `audit` schemas in one database; gold reached only through published views.
- dbt for transforms; an embedded, app-owned Airflow (scheduler, API server, own metadata database, local executor) with DAGs generated from manifests and dbt run through Cosmos; the Airflow UI is never exposed to users; the app talks to Airflow only through a thin orchestrator interface.
- Manifest (YAML) with required fields, a validator, licence register (OGL v3), known-issues register, run history, check results, audit log.
- Trust tiers (Certified, Usable with caveats, Exploratory only, Not for AI), a rule-based score proposing a tier, steward override with reason, automatic restore, owner promotion, steward sign-off for Certified, weakest-input rule for combined answers.
- Access levels Open, Group, Named (default Named), four roles (consumer, owner, steward, admin), admin-managed groups, named grants with optional expiry, the same rule on every door.
- Phase 0 doors: SQL on published views with read-only roles, file export (CSV, GeoPackage) with metadata sidecar and version file, an MCP server with search, describe and query tools.
- No AI-generated content in the platform; configuration in YAML; secrets from the environment; keep four bronze versions; audit log never holds query results.
- Build order: slices 0 to 12 of Build Brief 001, each with a Done-when list, and a 15-minute demo script.

**Left open by the sources:** trust-score signals, weights and thresholds; the minimum tier per use case and who sets it; how caveats are structured in MCP responses; Fresh/Late/Stale tolerances; whether stewards and admins see Group and Named data; whether a product's existence can ever be hidden; exact Airflow, Cosmos and dbt-postgres versions; the front-end stack of the app; how the MCP server identifies the user locally (no Entra in Phase 0); the synthetic product's geography; where the app finally runs inside BUUK.

## Problem and outcome
**As given:** publish BUUK's data as versioned, documented products that people and AI tools can find, query and rely on, with the trust tier and known issues travelling with every answer.

**Who has the problem and what it costs:** BUUK staff and the AI tools they are starting to use reach data as raw tables or files. Nobody can tell from the data itself how fresh it is, where it is weak, who owns it or whether AI may use it, so answers are either refused ("the data isn't trustworthy enough for AI") or given without caveats.

**The real problem:** consumers cannot tell how far to trust a dataset, because its meaning, quality, freshness and permitted use do not travel with it.

**Outcome that shows it is solved:** a person or an AI assistant can find a product, read its tier and known issues, and get data whose caveats arrive with it through every door, while access and licence rules are enforced in one place.

**Phase 0 outcome:** the Historic Flood Map runs from download to a published, tiered product with a product page, SQL, file export and an MCP query, plus a synthetic sites product for a cross-product spatial join; the demo script passes on a clean stack.

## Users and main workflows
| Who | How often | Device | Main things they do |
|---|---|---|---|
| Consumer (analyst, GIS user, AI assistant user) | Weekly | Desktop browser, SQL client, QGIS, VS Code or MCP Inspector | Search the catalogue; read a product page; query by SQL or MCP; download a file with its sidecar; report an issue |
| Owner (pipeline owner) | Per product, then at each refresh | Desktop, Git | Write the manifest and dbt models; review the generated page; fill missing fields; promote silver to gold |
| Steward | Weekly triage | Desktop browser | Triage open findings and reported issues; override a tier with a reason; sign off Certified |
| Admin | Occasionally | Desktop browser | Create users, groups and named grants; assign roles |
| The platform (scheduled) | Quarterly source, weekly version check | Airflow | Check the source version; ingest; build silver and gold; run checks; profile; score; report run history |

Phase 0 users: the build team and the demo audience; Phase 1: a small named pilot group.

## Scope
**In (first release = Phase 0, slices 0 to 12 of Build Brief 001):** the foundation stack; catalogue tables and manifest validator; ingest to bronze with version detection; silver and gold with dbt; checks and freshness; profiling and published gold views with metadata tables and read-only roles; trust score, tiers, override and promotion; DAG factory on embedded Airflow with Cosmos; sign-in with four roles, groups and named grants; catalogue list and five-tab product page; report-an-issue, steward list and missing-fields list; the synthetic sites product and the spatial join; the MCP server (search, describe, query); file export with sidecar and version file; the demo script and findings note.

**Out of the first release:** OGC API Features, REST and OData doors; the form-based pipeline builder and DAG graph; map previews; data requests, usefulness feedback and the API console; Entra sign-on and Copilot; lineage, subscriptions, alerting; separate environments; any AI-generated content; any storage other than Postgres; any BUUK internal data. All phases (1 to 3, and the parked Phase 4 items) are specified in full in the spec pack; Phase 0 is still the first build (Gate 1, D-007).

## Success measures
- Every slice's Done-when list passes as an automated test or a recorded check.
- The demo script runs start to finish on a clean stack with no manual fixes, in about fifteen minutes.
- The cross-product spatial join on the real flood map (21,578 multipolygons) runs in an acceptable time, or the findings note says where Postgres reaches its limit.
- A user outside a product's access level cannot read its data through the product page, SQL, MCP or export (tested per door).
- An MCP answer drawing on both products carries both products' caveats and the weaker tier.
- Nothing in the platform produced AI-generated content.

## Maturity tier
Tier 2, Personal tool, for the Phase 0 prototype; raised to Tier 3, Shared pilot, before the Phase 1 pilot (Gate 1, D-008). At Tier 3: several named people with different roles, access levels to enforce on four doors, and the prototype grows into the Phase 1 pilot. That means: a test per requirement and per door for access, an end-to-end demo journey, the security standard's tier 3 items (hashed passwords, named accounts, audit of sign-ins and permission changes, secrets from the environment), and documentation for the demo, the findings note and how to add a product. The data itself is public, which keeps the privacy load low.

## Deployment and runtime
Local Docker Compose on one host (DEF-02, and the source's hosting decision): Postgres 17 with PostGIS 3.5 (data and catalogue), a separate Postgres for Airflow's metadata, Airflow scheduler and API server (local executor), the web app (backend and front end), and the MCP server. Published ports bind to 127.0.0.1 (POL-06) and are chosen in the 52xx, 80xx and 54xx ranges after `docker ps`. The Airflow UI port is not published. The stack needs outbound internet to download public data. Expected memory: about 6 to 8 GB for the whole stack, within the 16 GB Docker Desktop gets on this machine.

## Interfaces and design
- **Web:** sign-in, catalogue list with search and filters (owner, tier, access level, spatial), five-tab product page (Overview, Data dictionary, Quality, Known issues, Access) under a header with tier, access level, freshness and owner; steward findings list; owner missing-fields list; admin users, groups and grants; run history.
- **Non-visual:** SQL endpoint (Postgres read-only roles on the gold schema), file export, MCP server over stdio and streamable HTTP for MCP Inspector and VS Code, Airflow DAGs generated from manifests.
- **Design:** unbranded, the template's design file, quiet UI (about-me). Tiers and freshness are shown as text labels, never by colour alone (DEF-08).

## Data, identity, integrations and security needs
- **Stored:** the flood map in bronze (four versions kept), silver and gold; a synthetic sites product; the catalogue (products, versions, manifests, licences, known issues, checks, runs, profiles, scores, users, roles, groups, grants); the audit log (12 months, never query results). Sensitivity: public and synthetic data only; user accounts (name, email, hashed password) are the only personal data.
- **Identity:** named accounts with four roles, admin-managed groups, named grants with expiry; Postgres login roles that mirror access levels for the SQL door; the MCP server acts for a signed-in user through a personal access token (ASM-004). Entra comes in Phase 3.
- **Integrations:** EA Historic Flood Map download (OGL v3, key-less). Sample saved: `source/samples/historic-flood-map-schema.md` and the version file. Fields kept: all of them (`objectid`, `shape_leng`, `layer`, `shape`). Finding: the file has almost no attributes, so the dictionary relies on the EA catalogue record and the owner, and freshness parses the version text "This is the September 2026 release." MCP clients: MCP Inspector and VS Code, both local.
- **What could go wrong:** caveats lost through raw SQL (mitigated by views with flag columns and metadata tables); an access check missed on one door (tested per door); MCP sending restricted data to a model provider (only AI-enabled, Open, public products reachable); an owner promoting their own failing product (gate plus audit plus steward override).

## Operations and support
Run by the owner with `docker compose up`; tests with one command. Airflow schedules the weekly source-version check and the quarterly refresh. Backups: a nightly dump of the data and catalogue database (DEF-06); bronze keeps four versions for rollback. Failures show in the app's run history and as known issues; alerting is Phase 2. The heartbeat option reports that the app is alive. Airflow, Cosmos, dbt-postgres and the MCP SDK are pinned with Airflow's constraints file.

## Template and candidate catalogue assets
**Recommended: `web-app` (CAT-006).** It already gives Postgres with PostGIS, multi-user sign-in, a FastAPI backend, a React front end, Compose and CI. **Runner-up: `web-app` plus `etl-pipeline`.** The etl-pipeline's runner, SQLite store and validate-quarantine stages are replaced by Airflow and dbt in the sources, so it would mostly be removed; its ingest-with-hash pattern can be borrowed without adopting the template.

**Gaps (new components, each to be imported into the catalogue and approved before use, POL-10):** Apache Airflow (Apache-2.0), Astronomer Cosmos (Apache-2.0), dbt-core with dbt-postgres (Apache-2.0), the official MCP Python SDK (MIT), and the `postgis/postgis` image (already used by the template). `fastmcp` is starred but not imported. CAT-054 (dbt-duckdb) and CAT-059 (dagster-postgres) do not fit.

**Composition hypothesis:** web-app backend (catalogue API, sign-in, product page data, export) + React front end + a `pipelines/` package (manifest validator, DAG factory, orchestrator interface, checks, profiling, scoring) + a dbt project + an MCP server sharing the backend's service layer + Airflow containers.

**Template features** (pre-filled from DEF-26, changed where this idea needs it): Share off (no public pages; data must not leave by link), Theme changer on, Help with Guides on, Feedback on with Problem, Suggestion and Question kept in the app's admin list (`store`), because no mail service is set up and a report-an-issue route already exists; Heartbeat on.

## Lenses
All eight active: business-value and users-and-workflows (always); data-and-integrations (several stores, an external source); architecture (many components); security (tier 3, access levels on four doors, MCP to a model provider); ux-and-accessibility (catalogue and product page); operations (scheduled pipelines, embedded Airflow); cost-and-maintainability (Airflow upgrade burden, open-source licences, the build-versus-adopt spike).

## Assumptions, risks and open questions
**Coverage scan:**
- scope: Clear (slices 0 to 12 with Done-when lists)
- users: Clear (four roles, pilot later)
- data: Partial (real file inspected; dictionary and freshness text need rules)
- integrations: Partial (EA source clear; MCP identity without Entra not decided)
- non-functional: Partial (spatial performance unknown; acceptable join time not defined)
- risks: Clear (sources list them)

**Assumptions** (the recommendation is taken unless the owner overrules it at Gate 1):
- ASM-001: Superseded at Gate 1: the spec covers all phases in full (D-007).
- ASM-002: The app's front end is the template's React front end; the source does not choose one.
- ASM-003: Airflow 3.x with the matching Cosmos and dbt-postgres releases, pinned with the constraints file; the exact versions are fixed in Spec after a check that they install together.
- ASM-004: MCP identity in Phase 0 is a personal access token created by the signed-in user in the app; the MCP server acts for that user. Entra replaces it in Phase 3.
- ASM-005: Stewards and admins do not see Group or Named data unless granted; owners always see their own; a product's existence is never hidden from signed-in users.
- ASM-006: First trust-score proposal: authority 25, freshness 20, checks 30, open issues 15, completeness 10; Certified at 85 and above, Usable with caveats at 60, Exploratory below; any open blocking issue caps at Exploratory; Not for AI is set by licence or manifest, not the score. All in YAML config.
- ASM-007: Freshness for a quarterly source: Fresh within 100 days of the source release, Late to 130 days, Stale after; the release month is parsed from the version text.
- ASM-008: Minimum tier per use case is passed by the MCP caller as a request parameter, with a configured floor (Exploratory for Phase 0).
- ASM-009: The synthetic sites product is about 200 polygons placed to overlap known flood map areas, generated by a seeded script and committed.
- ASM-010: "Acceptable" spatial-join time is under 10 seconds on the real data for one network type; the findings note records the actual figure either way.
- ASM-011: The build-versus-adopt spike is out of scope for this idea (the source says so).

**Risks:**
- RISK-001: Embedded Airflow is heavy for one host (several containers, memory, upgrade burden); mitigated by the local executor, pinned constraints and contract tests for the DAG factory.
- RISK-002: Airflow, Cosmos and dbt-postgres version compatibility; mitigated by fixing versions in Spec after an install check.
- RISK-003: Access enforced differently on one of four doors; mitigated by one shared access service and a test matrix per door and level.
- RISK-004: MCP responses go to the model provider behind the assistant; mitigated by AI use off by default and only Open, AI-enabled, public products reachable.
- RISK-005: The flood map's few attributes leave the data dictionary mostly owner-supplied, so completeness lowers the tier until filled; expected and shown as Not set.
- RISK-006: BUUK material sits in the factory repository; it stays private (DEF-14) and nothing is published without a typed confirmation (POL-01).
- RISK-007: Geometry repair and the spatial join on 21,578 multipolygons may be slow; measured and recorded in slice 9.

**Open questions:** answered at Gate 1 (see Clarifications).

## Decisions and overrides
- Spec depth: all phases in full, not Phase 0 in full with later phases outlined (D-007, also in `learned.md`).
- Tier: 2 for the prototype, 3 for the pilot, not 3 throughout (D-008, also in `learned.md`).

## Clarifications
### Session 2026-11-14
- Q: Spec depth? -> A: C, all phases in full (D-007)
- Q: Maturity tier? -> A: B, Tier 2 for the prototype, Tier 3 for the pilot (D-008)
- Q: New components (Airflow, Cosmos, dbt-postgres, MCP Python SDK)? -> A: A, import and approve all four in Spec (D-009)
