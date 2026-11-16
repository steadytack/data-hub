# 01 Overview

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-01 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

## What it does
Data Hub (Trusted Data Products) publishes BUUK's data as versioned, documented data products. An owner describes a product in a short manifest; the platform ingests it, cleans and checks it at each layer, profiles it, scores it into a trust tier, builds its product page, and serves it through SQL, file export and an MCP server (later OGC API Features, REST and OData), with the tier, freshness and known issues arriving with every answer. The first proof is the Environment Agency's Historic Flood Map plus a synthetic sites product joined to it.

## Why it is needed, and for whom
BUUK staff and the AI tools they use reach data as raw tables or files that say nothing about their freshness, weaknesses, owner or permitted use, so AI answers are either refused or given without caveats. Consumers (analysts, GIS users, AI assistant users), owners, stewards and admins all need one place where trust is stated and enforced.

## Main capabilities
- Publish from a manifest, with a licence register and a refusal for incomplete manifests (FR-002 to FR-005).
- Ingest to bronze with version detection, dbt silver and gold, checks at each boundary (FR-009 to FR-011, FR-018 to FR-022).
- Freshness, profiling, drift, trust score, tiers, override, restore, promotion and sign-off (FR-023 to FR-033).
- DAGs generated from manifests on an embedded Airflow, with run history in the app (FR-038 to FR-043).
- Catalogue, five-tab product page, report an issue, steward and owner lists (FR-050 to FR-060).
- Sign-in with four roles, groups and named grants, one access rule on every door (FR-066 to FR-073).
- MCP search, describe and query with combined caveats and the weakest tier; file export with sidecar and version file (FR-082 to FR-087, FR-094 to FR-096).
- Later phases: pilot hardening and review queue (Phase 1), more doors and a pipeline builder (Phase 2), Entra, Copilot and governance (Phase 3), parked items (Phase 4).

## Success measures
OBJ-001 to OBJ-009 in doc 02; for the first build: the demo script passes on a clean stack in about 15 minutes, access holds on every door, combined MCP answers carry both products' caveats and the weaker tier, and the spatial join on the real flood map runs under 10 seconds or the findings note says where Postgres reaches its limit.

## Scope
In: all phases specified (D-007); Phase 0 (Build Brief 001, slices 0 to 12) built first. Out: AI-generated content, storage other than Postgres, BUUK internal data until the data rules are agreed (a Phase 0 gate deliverable, CR-001), public access with no sign-in, the build-versus-adopt spike.

## Tier and deployment
Tier 2 for the Phase 0 prototype, Tier 3 from the Phase 1 pilot (D-008); local Docker Compose on one host, ports on 127.0.0.1.

## Reuse
Web-app template CAT-006 1.4.0 (adapted) for the backend, front end, PostGIS database and backups; gitleaks CAT-056. Imported and approved at Gate 2: Apache Airflow CAT-100, Cosmos CAT-101, dbt-core CAT-102, dbt-postgres CAT-103, the MCP Python SDK CAT-104. Built here: manifest validator, DAG factory, checks and trust scoring, access service, GeoPackage module, MCP tools. Detail in doc 12.

## Key risks, assumptions and decisions
RISK-002 (Airflow and dbt compatibility, eased by DEC-004), RISK-003 (uneven access across doors), RISK-004 (MCP data reaching a model provider, DEC-007), RISK-007 (spatial performance), RISK-010 (libraries approved on a static review). ASM-006 and ASM-007 set the first score and freshness thresholds. Detail in doc 09.

## Delivery and next gate
Phase 0: EPIC-001 to EPIC-009 (slices 0 to 12); Phase 1: EPIC-010, EPIC-011; Phase 2: EPIC-012 to EPIC-015; Phase 3: EPIC-016 to EPIC-018; Phase 4 parked: EPIC-019, EPIC-020. Next gate: Gate 2 (this pack), then Build from EPIC-001.

## Glossary
| Term | Meaning |
|---|---|
| Access level | Who can see a product's data: Open (everyone with an account), Group (members of chosen groups) or Named (named people and the owner). Not the same as the trust tier |
| Airflow | Apache Airflow, the workflow scheduler embedded in the app that runs each product's pipeline |
| ArcGIS | Esri's desktop and web mapping software |
| Asset (Airflow) | Airflow's name for a dataset a DAG produces; another DAG can run when it updates |
| BI | Business intelligence: reporting tools such as Power BI and Excel |
| Bitol | An open standard for data contracts and data products, which the manifest may later align with |
| Bronze, silver, gold | The medallion layers: bronze holds raw loads unchanged, silver cleaned and typed data, gold the published product |
| BUUK | The organisation this platform is built for, and the owner of the source documents |
| Copilot | Microsoft 365 Copilot, the AI assistant most BUUK staff use; Copilot Studio builds agents for it |
| VM | Virtual machine: a server running in BUUK's data centre or a cloud, where the pilot runs and where some sources sit |
| AWS | Amazon Web Services, a cloud that may host some source systems |
| Cosmos | Astronomer Cosmos, a library that runs each dbt model as its own Airflow task |
| CRS | Coordinate reference system: how coordinates map to places on the earth |
| CSRF | Cross-site request forgery: tricking a signed-in browser into sending a request |
| DAG | Directed acyclic graph: an Airflow pipeline definition, its tasks and their order |
| Data version file | A small text file stating which version and date a dataset or download is; the source publishes one, and every export carries one |
| dbt | A tool that builds tables and views from SQL models and tests them; here dbt-core with the dbt-postgres adapter |
| DBeaver | A desktop SQL client |
| EA | The Environment Agency, the English public body that publishes the Historic Flood Map |
| Entra | Microsoft Entra ID, Microsoft's identity service, used for BUUK sign-on in Phase 3 |
| EPSG | The register of coordinate systems; EPSG:27700 is the British National Grid |
| Freshness | Whether data is as current as its schedule says it should be: Fresh, Late, Stale or Unknown |
| GDAL | A widely used library and command-line toolset for reading and writing geospatial formats; used here only in tests |
| GEMINI | The UK's metadata standard for geospatial data, a profile of ISO 19115 and ISO 19139 |
| GeoPackage | An open file format for geospatial data, held in a SQLite database |
| GIS | Geographic information system: mapping software such as QGIS and ArcGIS |
| Historic Flood Map | The EA dataset of the outlines of all recorded flooding in England, released quarterly |
| IT | BUUK's information technology and security teams, who approve AI tools and network access |
| ISO 19139 | The XML encoding of the ISO 19115 geographic metadata standard |
| JWT | JSON Web Token: the signed token that keeps a web session |
| Known issue | A register entry describing something wrong or uncertain in a product's data, specific enough for an agent to act on |
| Licence register | One entry per licence (terms, attribution, permitted uses, AI use); products link to it |
| Manifest | The YAML file that defines a product and its pipeline: owner, source, licence, schedule, access level, AI use and checks |
| MCP | Model Context Protocol: an open protocol through which AI assistants call tools on a server |
| MCP Inspector | The official developer tool for calling an MCP server's tools and seeing the raw responses |
| Not set | What a page shows for a field nobody has filled |
| OData | Open Data Protocol: a standard for queryable data feeds that Power BI and Excel read |
| ODI | The Open Data Institute, whose Data Spectrum (closed, shared, open) the access levels simplify |
| OGC | The Open Geospatial Consortium; OGC API Features is its web standard for serving map features |
| OGL | The Open Government Licence v3, under which the flood map is published |
| OpenLineage | An open standard for lineage events that pipelines emit |
| Owner | The person accountable for a product: writes its manifest, fills gaps, promotes it |
| Personal access token | A secret a user creates in the app so a tool (such as an MCP client) can act as them |
| PostGIS | The spatial extension to PostgreSQL |
| Power BI | Microsoft's reporting tool |
| Product page | A product's page in the app: a header and five tabs (Overview, Data dictionary, Quality, Known issues, Access) |
| Promotion | The owner's step that publishes a passing silver product to gold |
| Published view | The gold-schema view consumers query; never a source table |
| QGIS | A free desktop GIS |
| SELECT | The SQL statement that only reads data; the MCP query tool accepts nothing else |
| Read-only login | A user's own database login that can read only published views and metadata tables |
| Sidecar | The metadata file that travels inside every download: tier, known issues, version and licence attribution |
| Steward | The person who triages issues, overrides tiers with a reason and signs off Certified |
| Trust score | A number from 0 to 100 computed from authority, freshness, checks, open issues and completeness, which proposes a tier |
| Trust tier | The label people and agents see: Certified, Usable with caveats, Exploratory only or Not for AI |
| VS Code | Visual Studio Code, an editor whose agent mode can connect to MCP servers |
| Weakest-input rule | A combined answer's tier is the lowest tier among the products it used |
| WMS, WFS | Older OGC web map and web feature services, parked for later |
