# Trusted Data Products: Consolidated Spec (v0.5)

Nov 13, 2026 · @Tom Duncan

A trusted data products platform publishes BUUK's data as versioned, documented products that people and AI tools can find, query and rely on, with the trust tier and known issues travelling with every answer.

This version merges the v0.4 access and trust spec with the v0.5 pipelines section. The first build is Postgres-first: one Postgres database with PostGIS, dbt for transforms, and an embedded Airflow for orchestration. Consumers reach published products through MCP, OData, SQL, REST, OGC API Features and file export.

**Redrafted:** the original v0.3 base spec could not be found. Its product page, known-issues register, licence register, freshness tracking and OData access were rebuilt from earlier design notes, and are marked as redrafted where they appear.

## Why a data product, not a database connection

A database connection gives AI access, but a data product gives it understanding and limits. A database hands an AI rows with no context. A data product hands it the rows plus everything a careful analyst would know: what the columns mean, where the data is weak, how fresh it is, and what it may be used for.

1. **Meaning.** Each product carries a data dictionary, units and definitions, so the AI does not have to guess what a column called `status` or `dia` holds, or whether a diameter is in millimetres or inches.
2. **Honesty.** Known issues and the trust tier are returned with every answer, so caveats cannot be separated from the data. An AI can say it can answer, but material is unknown for some pipes in this area.
3. **Safety.** Permissions, row limits and licence rules, including whether AI use is allowed, are enforced centrally rather than left to each tool.
4. **Stability.** A published product is a versioned contract, so an AI tool or a Power BI report does not silently break when the source changes.
5. **Accountability.** Every question an AI asks, and every dataset it relied on, is logged, so a challenged answer can be traced to the product, version and caveats.

**A feedback loop a database never has.** When an AI tool or a user hits a flaw, they report it into the known-issues register. A steward triages it, it is measured against a rule, and it is tracked to a fix at source, so the data improves for everyone.

> Connecting AI straight to a database gives it access. Publishing a data product gives it understanding and limits: what the data means, where it is weak, what it is allowed to be used for, and a record of what it relied on. That is what turns "we can't trust it for AI" into "we know exactly how far to trust it."

## Trust is a gradient, not a yes or no

The aim is to move past "the data isn't trustworthy enough for AI" by being specific about what each dataset is fit for. Each product carries a small set of machine-readable signals:

- **Authority:** who owns it and whether it is the authoritative record.
- **Freshness:** how fresh it is against how fresh it claims to be.
- **Checks:** which automated quality checks passed, against what baseline.
- **Known issues:** written specifically enough for an agent to act on, for example "pipe material is null for about 12% of records, mostly pre-1970 mains".
- **Licence:** whether redistribution and AI use are permitted.

### Trust tiers

| Tier | Meaning | Typical use |
| --- | --- | --- |
| Certified | Authoritative, issues documented and within baseline | Customer-facing or decision-grade answers |
| Usable with caveats | Authoritative or reliable, but with material known issues | Internal analysis, with caveats returned |
| Exploratory only | Unverified or partially checked | Prototyping and discovery, never quoted as fact |
| Not for AI | Licence, quality or sensitivity rules forbid it | Excluded from MCP responses |

Each use case sets the **minimum tier** it will accept. A customer-facing answer might require Certified; internal analysis might accept Usable with caveats.

**Trust score underneath the tiers.** Each product also gets a numeric score computed automatically from its signals: authority, freshness, check results, open known issues and completeness of human-supplied fields. The score proposes a tier, and the tier is the label people and agents see. A steward can override a proposed tier, with the reason recorded.

**Combined answers.** When an answer draws on more than one product, it carries the caveats of all of them, and its effective tier is that of the weakest input.

**What Certified requires.** A product can only be Certified if its checks pass, its manifest is complete and a steward has signed it off.

## Publishing and auto-generation

The platform is technology-agnostic on the producer side. A product may be built in Databricks, Python, SQL, an API pull from an external system, or an uploaded CSV. The platform only cares what arrives at the door: the data plus a short manifest. It then builds the library page itself.

### The publish contract

Every product is published with a manifest, and publishing is refused without the required fields.

- **Owner:** the accountable person or team.
- **Source:** where the data comes from, internal or external.
- **Licence:** for external data, a link to its entry in the licence register, which holds the upstream terms.
- **Refresh schedule:** how often it is expected to update, which drives the freshness measure.
- **Access level:** Open, Group or Named. It starts as Named until the owner chooses a broader level.
- **AI use:** whether AI tools may use the product. It is off unless the licence permits it and the owner switches it on.

### What the platform builds itself

| Part of the page | Source | Status |
| --- | --- | --- |
| Column names, types, ranges, null rates, distinct values | Profiling the data on arrival | Automatic |
| Row counts, freshness, completeness | Profiling plus the refresh schedule | Automatic |
| Geometry type and coordinate system (spatial) | Profiling | Automatic |
| Column descriptions | Harvested from source metadata where it exists | Human-supplied where not harvested, flagged if missing |
| Quality checks and issue findings | Automatic issue detection from the known-issues model | Automatic, steward triage |
| Owner, licence, upstream source | Manifest | Human-supplied |
| Business definitions and units | Steward or owner | Human-supplied, flagged if missing |

Each tab on the product page is a view over three feeds: the manifest, the profiling results, and the live issue and check registers. There is no hand-built page per product.

**Harvest first, flag the gaps.** If the source already carries metadata, such as a Databricks catalogue entry, a file schema or an API specification, the platform harvests that first and only flags what is missing for the owner to complete.

**Change over time.** Profiling runs on every refresh, not just the first publish. Drift in schema, null rates or value ranges is compared against the baseline, and regressions raise a finding for the steward rather than changing anything silently. Missing human-supplied fields, such as units or definitions, lower the product's trust tier until they are completed.

### Automation with a human in the loop

The platform produces what it can by rule, from the data itself, source metadata, run history and checks, and a person supplies the rest. For now it does not generate descriptions or other content with AI. This list of fields comes from reviewing the Environment Agency, Natural England and UK Power Networks open data pages.

| Field | What it holds | How it is produced | Human role |
| --- | --- | --- | --- |
| Authority basis | The legal or business basis for being the authoritative record | Harvested from source metadata where it states one | Owner supplies it if none is found |
| Dates | Created, last revised, published, last run and next expected | Source metadata, run history and the refresh schedule | None |
| Standard metadata | Spatial reference, extent, responsible party, contacts, language and the ISO 19139 file | Profiling, the manifest and the generated metadata file | None. Missing values show as Not set |
| Data version file | A small file stating which product version and date a download is | Generated each run | None |

The rules that keep humans in the loop without making them the bottleneck:

- **Rules first.** Everything the platform produces comes from profiling, harvested source metadata, run history and checks. It does not generate descriptions or other content with AI at this stage.
- **Approval depends on the tier.** Exploratory products publish without review. Usable with caveats needs the owner to confirm. Certified needs steward sign-off.
- **People see exceptions, not everything.** A review queue holds only missing fields and flagged items, each with its evidence, and one-click accept, edit or reject. Similar items can be approved in bulk.
- **Re-review only what changed.** A schema change, a new licence or a new source version reopens just the affected fields. The owner gets reminders and escalation before a missing or overdue field lowers the tier.
- **Never silent.** Every automatic decision is logged and reversible, and the audit log shows who confirmed what and when.

## Product page and registers

*Redrafted. The original v0.3 text for these sections was not recovered, so this is rebuilt from the design notes in earlier chats and should be reviewed as new.*

### The product page

Each product has one page with five tabs. Every tab is a view over the manifest, the profiling results and the live registers, so nothing is hand-built per product. The trust tier, access level, freshness and owner sit in a header above the tabs.

| Tab | What it shows |
| --- | --- |
| Overview | Description, owner, authority status and basis, licence and attribution, version, tier, freshness and dates |
| Data dictionary | Columns, types, units and definitions, with missing entries flagged |
| Quality | Check results against the baseline, profiling figures and drift since the last refresh |
| Known issues | Open and resolved entries from the known-issues register, with a report-an-issue button |
| Access | Data preview, downloads, copyable connection details for OData, SQL, REST and OGC API, and a ready-made MCP snippet |

### Known-issues register

The register records what is wrong or uncertain about a product, separately from whether the product is the authoritative record.

- **Authority** is a property of the product: whether it is the authoritative record, a derived copy, a reference copy or third-party data, and who owns it.
- **Quality** is a property of the data: how complete, accurate, consistent, valid and timely it is. Quality problems are the register entries.

The two are scored separately. A product can be authoritative but have material quality issues, so it is Usable with caveats. Another can be clean but not authoritative, so it should not be quoted as the record.

Each entry holds:

- **Scope:** the product, versions, columns, areas or record types affected.
- **Description:** written specifically enough for an agent to act on, for example "pipe material is null for about 12% of records, mostly pre-1970 mains".
- **Type and severity:** the quality dimension affected, and whether it is minor, material or blocking.
- **Measure:** the rule that detects it and its current value, so it can be tracked over time.
- **Origin:** found automatically at publish or refresh, reported by a user or an AI tool, or raised by a steward.
- **Status and owner:** open, triaged, accepted, being fixed or resolved, with the person responsible and a link to the fix at source.

Entries move through a fixed path: detected, triaged by the steward, measured against a rule, tracked to a fix at source, then resolved. Automatic detection runs at publish and on every refresh. Severity feeds the trust score and caps the tier, and open entries are returned with every MCP and OData response.

### Licence register

External datasets carry upstream terms, so each has one entry in a central register, and a product links to its entry instead of copying the terms.

- **Terms:** licence name and link, attribution wording, and any share-alike or derivative limits.
- **Permitted use:** redistribution, commercial use, internal-only, and whether AI use is allowed.
- **Ownership and dates:** who manages the relationship, and any expiry or renewal date.

The register is enforced, not just recorded. A product whose licence forbids AI use is Not for AI. A product with an unknown licence is held at Exploratory only until reviewed. Attribution wording and licence terms are written into every download's metadata sidecar.

### Freshness tracking

Freshness compares how current the data is against how current it is expected to be.

- **Expected:** the refresh schedule in the manifest.
- **Actual:** the dates are tracked separately: when the data was created, its last revision (the as-of date), when it was published, and when the pipeline last ran. A pipeline can run on time and still load old data, so these are never merged into one. For external datasets, the upstream publication date is used where it is available.
- **State:** Fresh, Late (past due but within tolerance) or Stale (beyond tolerance).

A Stale product is a blocking check, so it opens a known issue and lowers the tier. The freshness state is shown in the product header and returned with every MCP and OData response.

### Requests, feedback and trying the API

Three features from the reference portals are in scope for the first version.

- **Data request form:** anyone can ask for a dataset that is not in the catalogue. Requests are logged, the platform checks whether a matching product or an open request already exists, and a steward triages the rest into a backlog.
- **Usefulness feedback:** each product page has a quick "was this useful?" prompt, separate from report-an-issue. Low scores and free-text comments are grouped by product and shown to the owner.
- **API console:** the Access tab includes a try-it console for the REST and OGC API endpoints, using the signed-in user's own permissions, with the same limits as any other query.

## Pipelines and orchestration

Pipelines are how a data product earns its trust tier: every layer of the medallion architecture produces the checks, freshness and issues that the catalogue and MCP layer expose.

### Decisions made

- **Pipelines are code, and a UI can build that code.** The manifest and pipeline definition live in Git. The UI is an editor that reads and writes them, so an engineer and a first-time user end up with the same artefact.
- **The pipeline owner promotes silver to gold.** Stewards are not in the promotion path. They keep their role on known issues and tier overrides.
- **Airflow is embedded in this app.** It is not a separate or shared instance, and it is not BUUK's existing Airflow.

### Medallion layers and trust tiers

Only gold is a data product. Bronze and silver are internal working layers that consumers never see through the catalogue, OData or MCP.

| Layer | What it holds | Typical tier | Exposed to consumers |
| --- | --- | --- | --- |
| Bronze | Raw ingest, unchanged from source, ingested once and shared across pipelines | Exploratory only | No |
| Silver | Cleaned, typed and conformed (default CRS EPSG:27700, standard keys) | Usable with caveats | No |
| Gold | Published, versioned data product with manifest, dictionary and known issues | Certified, or Usable with caveats | Yes |

The tier is still set by the trust score and steward overrides. The layer only caps what is possible: a product cannot be Certified if its pipeline has no passing silver-to-gold checks. In the first version, each layer is a separate schema in one Postgres database with PostGIS, and gold is exposed to consumers only through published views.

### Pipeline manifest and UI builder

A pipeline is defined by a manifest file in Git, extending the product manifest. The platform generates the Airflow DAG from it, so nobody hand-writes DAGs. The manifest declares:

- **Sources:** where raw data comes from, with ingest into bronze happening once and shared across pipelines.
- **Transforms:** the dbt models for bronze to silver and silver to gold, run in Postgres with PostGIS.
- **Checks:** dbt tests, freshness thresholds, row-count and schema-drift rules for each layer boundary.
- **Schedule or trigger:** a cron schedule, or an upstream product that triggers a refresh.
- **Owner and product metadata:** pipeline owner, licence, dictionary and the GEMINI fields already required.

The UI is a form-based editor with live previews. It reads and writes the manifest and the dbt model files, commits them to Git, and shows the generated DAG read-only. An engineer can skip the UI and edit the files directly, and the UI picks up the change, which keeps one source of truth.

### Pipeline builder UI

The first version uses forms, not an editable graph canvas. The interaction is modelled on the QGIS Processing toolbox: a searchable list of steps, each opening as a form with its inputs, parameters and output, and help text beside it.

- **Step library:** a searchable list of preset steps, grouped as Ingest, Clean, Spatial, Check and Publish. Each preset generates SQL. A custom SQL step covers anything the presets do not.
- **Step form:** each step takes a dataset or the output of the previous step, plus its parameters, and shows a live preview and row count before anything is saved.
- **Pipeline view:** steps are chained in order, Source, Clean, Checks, Publish, and can be reordered. Each step shows the SQL it generates, read-only by default and editable on request.
- **Orchestration in the same form:** schedule, trigger and upstream products are manifest fields, not something drawn. The generated Airflow DAG is shown as a read-only graph with live status on each task, run history, and rerun and retry buttons.
- **Run log:** each run has a log per step, as in QGIS, and a failure links back to the step that caused it.
- **Saved as files:** the UI writes the manifest and the dbt model files to Git, so a pipeline built in the UI and one written by hand are the same thing.

The difference from QGIS is that a Processing tool runs once, interactively. Here the steps are saved as a pipeline that runs on a schedule, is checked at each layer boundary and ends in a published product.

Later, an editable graph canvas can be added on the same manifest, in the way QGIS's graphical modeler sits on top of the same tools. Nothing built in the first version would be thrown away.

### Embedded Airflow

Airflow ships as a component of the app and is deployed with it. Users never open the Airflow UI. They use the app's own run history, which reads from Airflow.

- **Dedicated instance, owned by the app.** It is versioned and upgraded with the app, and does not share a metadata database, scheduler or DAG folder with any other Airflow at BUUK.
- **DAGs are generated, never hand-written.** A DAG factory reads each manifest and builds the DAG.
- **Assets for data-aware scheduling.** Each product is an Airflow Asset, so downstream products refresh when upstream ones update.
- **Cosmos for dbt.** Each dbt model runs as its own task, so failures and lineage show per model.
- **A thin orchestrator interface.** The app talks to Airflow only through manifest in, run status and check results out, so the engine stays replaceable.

This reverses the earlier Dagster decision. Airflow needs a little more glue than Dagster's asset model, and the app takes on running Airflow itself.

### Quality gates and promotion to gold

The pipeline owner promotes a product from silver to gold, and the platform only allows it when every gate passes.

1. Silver-to-gold checks pass against the baseline in the manifest.
2. The manifest is complete: owner, licence, update frequency, abstract, contact and a dictionary entry for every column.
3. Freshness is within the threshold the manifest declares.
4. The pipeline owner confirms the promotion, and the action is written to the audit log.

A failed check does not stop the last published version from being served. It opens a known issue automatically, with the failing rule and the affected columns, and the trust score can downgrade the tier until the issue is resolved. Because owners promote their own products, the audit log and the steward's tier override are the checks on that decision.

### Reporting run results to the catalogue

The catalogue is the source of truth for trust. Airflow is the engine that produces the evidence. After each run, the pipeline reports back through the orchestrator interface:

| Signal | Feeds | Shown to |
| --- | --- | --- |
| Last successful run and data timestamp | Freshness | Product page, MCP response |
| Check results per layer boundary | Checks, trust score | Product page, MCP response |
| Failed rules and affected columns | Known-issues register | Product page, MCP caveats |
| Schema changes between runs | Drift alert, version bump | Owner, consumers of the product |
| Run history and who promoted | Audit log | Owner, stewards |

## Access: how people and AI reach products

One published layer, a small set of doors. Whichever door is used, the trust tier and known issues come through with the data.

| Door | Used by | Notes |
| --- | --- | --- |
| MCP | AI assistants and agents | Few focused tools; caveats returned with every response |
| OData | Power BI, Excel, Power Query | Same products and caveats as the human catalogue |
| SQL | Analysts, notebooks, BI tools | Published views only, read-only roles |
| REST API | Internal apps such as the EAM platform, scripts | Non-spatial products; versioned |
| OGC API Features | QGIS, ArcGIS, GIS consumers | Spatial products; preferred over WFS as newer and simpler, with WFS only if legacy tools need it |
| File export | Data scientists, ad hoc use | CSV, and GeoPackage for spatial products. Every download carries a metadata sidecar and a version file |

Parked for later: an Arrow Flight endpoint, a Python SDK, and WMS or WFS services.

**Downloads and caveats.** A file loses its context once it leaves the platform, so each download ships with a small metadata sidecar carrying the trust tier, known issues, product version and licence. Only CSV and, for spatial products, GeoPackage are offered at first.

**Version file.** Each download also includes a small data version file stating which product version and date it is, so the file can be traced without returning to the platform.

### How this shows up in the MCP layer

- Tools stay few and sensible: search products, describe a product (schema, quality, known issues, tier), and query with limits.
- Every query response returns the data **and** its caveats and tier together.
- The MCP layer enforces the same permissions and licence rules as the catalogue.
- Calls are logged for audit, and a report-an-issue tool feeds the known-issues register.

**Testing the MCP server locally.** The server can be tested without any Microsoft 365 licence, domain or sign-in service, using two tools. **MCP Inspector** is the official developer tool for MCP servers. It gives a web page to call each tool and see the raw responses, including the tier and caveats, and it connects to a local server over stdio or HTTP. **VS Code with GitHub Copilot agent mode** connects to the same server through the "MCP: Add Server" command, using stdio or HTTP, so a real assistant can be asked questions and its handling of the caveats checked.

### SQL access for traditional tools

Analysts will also want to write plain SQL from tools like DBeaver, Python notebooks or Power BI's SQL connector, so the platform offers a SQL endpoint alongside MCP and OData.

- Consumers query **published views** in a dedicated schema, never source tables, with read-only roles scoped to that schema.
- Trust tier and known issues are discoverable in the same place, as metadata tables or column comments.
- Record-level quality flag columns let a query filter out suspect rows explicitly.
- Permission and licence rules apply as they do elsewhere, using row-level security, with row limits for AI-driven queries.

The risk to design against: raw SQL is where caveats are easiest to lose, and direct backend access to a database bypasses the tier and licence checks entirely.

### OData access for Power BI and Excel

*Redrafted from the design notes; the original v0.3 detail was not recovered.*

OData lets Power BI, Excel and Power Query read published products as live feeds without writing SQL. It must not become the route that strips caveats away, because a plain table feed carries no context.

- **One feed per product:** each published view is exposed as an OData v4 entity set, read-only, with sign-in through Entra.
- **Caveats travel with the feed:** a metadata entity set per product returns its tier, freshness, known issues, version and licence, and record-level quality flag columns are included so suspect rows can be filtered.
- **Server-side limits:** standard query options such as select, filter, top and paging are supported, with row limits and rate limits enforced by the platform.
- **Same rules as everywhere else:** permissions and licence rules are applied identically, and a product that is Not for AI or restricted to internal use is not offered to tools that would break those terms.
- **Spatial products:** OData exposes the attribute data, with geometry as text if needed. Mapping use goes through OGC API Features, since Power BI and Excel do not handle geometry well.

### Cross-cutting concerns

- **Security:** one identity across every door, ideally single sign-on for people, plus tokens or API keys for machines and agents. Permissions and licence rules are enforced identically everywhere.
- **Usage logging and rate limits:** record who pulled what through which door, and protect the platform from runaway queries, especially AI-driven ones.
- **Versioning:** published products are versioned contracts, so a change does not silently break REST, OGC or SQL consumers.
- **Change notifications:** consumers should find out when a product is updated, deprecated, or has a quality regression flagged, through webhooks or a subscribe-to-changes feature. Likely a later addition, but the product version and issue register should support it from the start.

### How to connect to a product

Each product's Access tab shows the connection details for every door that applies, ready to copy, so nobody has to remember these steps.

| Door | Phase | Tool | How to connect |
| --- | --- | --- | --- |
| Viewer in the platform | 1 | A web browser | Sign in, open the product, and use the data preview and the Access tab. A map preview for spatial products arrives in Phase 2 |
| SQL | 1 | DBeaver, Python notebooks, Power BI's PostgreSQL connector | Use the database address, port and read-only login from the Access tab, and query the published views. The tier and known issues are in the metadata tables beside each view |
| File export | 1 | Excel for CSV, QGIS or ArcGIS for GeoPackage | Choose Download on the Access tab. The sidecar and version file come with the file. GeoPackage is offered for spatial products only |
| MCP | 1 | MCP Inspector, VS Code with GitHub Copilot agent mode. Microsoft Copilot in Phase 3 | Add the server address in the tool's MCP settings, or with "MCP: Add Server" in VS Code. The assistant can then search, describe and query products |
| OGC API Features | 2 | QGIS, ArcGIS | Add the service address as an OGC API Features connection, then add the product's layer to a map |
| REST API | 2 | Internal apps, scripts | Call the versioned address with an API token. Each response carries the tier and caveats with the data |
| OData | 2 | Power BI, Excel, Power Query | Choose Get Data from an OData feed, paste the product's feed address and sign in. The metadata entity set appears beside the product's |

## Decision: no built-in chat, connect through MCP

The platform does not build its own chat interface at first. Users reach the data through AI tools they already have, connected to the platform's MCP server.

- **Chat is a moving target.** Staff tools improve constantly, and a built-in chat would mean maintaining a model integration, conversation handling and a UI that lags the real products.
- **The differentiator is the trust layer.** If the MCP server returns tiers and caveats properly, any assistant becomes a good front end.
- **It keeps the platform small.**

A scoped ask-a-question box on each product page is a possible later feature. It would sit on top of the platform's own MCP server, so it costs little if the foundations are right.

### Reaching users on Microsoft 365 Copilot

Most BUUK staff use enterprise Microsoft 365 Copilot, with some on premium licences. As far as this review found, M365 Copilot does not let an ordinary user add an MCP server directly. The route is a Copilot agent that wraps the platform's MCP server, which staff then use from Copilot or Teams.

- **Copilot Studio:** Microsoft's low-code agent builder can connect to a remote MCP server through a wizard.
- **Declarative agent:** built with the Microsoft 365 Agents Toolkit in VS Code, pointed at the MCP server address, with a chosen subset of tools exposed.

Requirements that follow for the platform: the MCP server must be remote, over HTTPS, using the streamable transport, as the older SSE transport is no longer supported. Authentication would go through Microsoft Entra, consistent with the single sign-on aim. More capable assistants, such as Claude or Copilot in VS Code, can connect to the same server for those who have them.

## Worked example: critical buildings inside gas site polygons

Two published products: **Critical buildings** (hospitals, schools, government buildings and similar sites supplied on BUUK networks) and **Site polygons** (the boundaries of BUUK's network sites). A user asks a chat assistant: "Return all critical buildings that are inside a gas polygon."

1. **Sign-in.** The user connects the assistant to the platform's MCP server and authenticates, so permissions and licence rules apply to everything that follows.
2. **Discovery.** The assistant searches the catalogue and finds both products. For each it reads the description, schema, trust tier and known issues.
3. **Trust check.** If both are Certified, it proceeds. If one is Usable with caveats, for example polygon boundaries are only accurate to a few metres, it proceeds and carries that caveat into the answer. If a product is below the minimum tier for the use case, it says so instead of answering.
4. **Compatibility check.** It confirms the two products share a coordinate system, have suitable geometry types, and that the polygons have a network type field to pick out gas.
5. **Query.** It runs a spatial query, critical buildings within gas site polygons, through the query tool, which enforces row limits and permissions.
6. **Answer with caveats.** The result comes back with the caveats attached, for example: "42 critical buildings fall inside gas site boundaries. The buildings layer is certified; the polygon layer has a known boundary-accuracy issue in this area."

### What this requires of the platform

- **Spatial joins across products** are a supported query pattern, so the query layer needs spatial functions, which PostGIS provides.
- **Combined caveats:** when an answer draws on more than one product, it carries the caveats of all of them, and its effective trust tier is that of the weakest input.
- **Compatibility metadata** such as coordinate system, geometry type and network type is part of each product's described schema, so an agent can check fit before querying.

## Worked example: publishing the Historic Flood Map as an internal product

The Environment Agency's Historic Flood Map is a good test of the whole publishing path. It updates quarterly, uses the British National Grid (EPSG:27700), is released under the Open Government Licence, and is available as a GeoPackage download and as an OGC API.

1. **Register the source.** Add the Open Government Licence to the licence register with the Environment Agency's attribution wording. Record the publisher, the source link and the quarterly update frequency.
2. **Define the pipeline.** The pipeline owner fills in a manifest, through the form UI or as a file in Git. It names the source, the schedule, the owner and the checks.
3. **Land the raw data (bronze).** An Airflow task downloads the GeoPackage unchanged into a bronze schema in Postgres. It records the download time, the link, the source's own version file and a file hash. If the source version has not changed since the last run, it stops.
4. **Clean it (silver).** dbt models standardise column names and types, confirm the coordinate system, repair invalid geometries and add standard keys.
5. **Check it.** Checks run at each layer boundary: row count against the last run, geometry validity, required columns, schema drift, and freshness against the quarterly schedule.
6. **Build the page.** The platform profiles the data and harvests the Environment Agency's own metadata, including the abstract, extent, contact, update frequency and licence, to fill the product page. Missing column descriptions are flagged for the owner.
7. **Publish a view (gold).** A dbt model creates a published view with quality flag columns, and the trust score proposes a tier.
8. **Promote it.** The pipeline owner reviews the generated page and promotes the product once the checks pass and the manifest is complete. Certified also needs steward sign-off.
9. **Serve it.** The product appears in the catalogue and is reachable through MCP, SQL, OGC API Features and file export. Downloads carry the licence attribution and a version file.
10. **Keep it current.** A weekly check looks for a new source version, and a new version triggers a rerun. Drift raises a finding, and a failed check opens a known issue and can lower the tier.

### Where people are involved

A person is involved at step 2, when the owner defines the pipeline, and at step 8, when the owner promotes it. Beyond that, the owner fills any column description gaps, and a steward signs off Certified. Everything else runs without intervention.

### Other sources

- **UKPN National Energy Outage:** the same steps apply, but the source refreshes every 15 minutes, so the pipeline polls an API on a short cycle and the stale threshold is minutes or an hour, not months.
- **Sites of Special Scientific Interest:** this should be a one-off polygon load with periodic checks for changes. What download or API the source offers still needs to be confirmed.

## Prior art: what exists and what to borrow

Open-source projects already cover part of this ground. The stance is to borrow good ideas freely and build the thin layer ourselves where that is simpler than adopting and operating a full platform.

| Project | What it offers | Idea worth borrowing |
| --- | --- | --- |
| [OpenMetadata](https://github.com/open-metadata/openmetadata) | Catalogue with built-in profiler, quality tests, 130+ connectors, and an MCP server for AI agents | Profiling metrics set (row counts, null proportions, uniqueness, trends); exposing context and trust to agents over MCP |
| [DataHub](https://github.com/datahub-project/datahub) | Metadata graph with streaming updates, GraphQL API, and an MCP server | Event-driven metadata updates, which support the change notifications in this spec |
| [Entropy Data](https://barc.com/de/analysteneinschatzung-entropy-data/) (formerly Data Mesh Manager) | Data product marketplace with a studio for owners, governance and access requests, built on the open Bitol data contract and data product standards. Its Data Contract CLI is open source | Contracts as the basis for the marketplace; access-request workflow; testing contracts in CI |
| [Data Product Portal](https://www.dataminded.com/de/resources/the-missing-piece-to-data-democratization-is-more-actionable-than-a-catalog) | Open-source portal for data products, positioned as a portal rather than a catalogue | Showing contract detail that catalogues usually hide. Current activity not yet checked |
| Amundsen | Lightweight search-led catalogue | None. Archived and no longer maintained |

Commercial platforms in the same space, reviewed for ideas only:

| Platform | What it offers | Idea worth borrowing |
| --- | --- | --- |
| [Atlan](https://atlan.com/data-marketplace/) | Marketplace of certified data products with built-in trust signals, ownership and lineage | Producer publishes once, consumer trusts what they find |
| [Ataccama](https://www.ataccama.com/platform/data-catalog) | Automatic trust score from quality and governance signals, served to AI agents over MCP | A numeric trust score computed underneath the friendlier tier labels |
| [Databricks Unity Catalog](https://www.databricks.com/blog/building-high-quality-and-trusted-data-products-databricks) | Tags and markdown to mark certification status, optional marketplace listing; an open-source version exists | Certification as a visible status on the product |
| [Collibra](https://www.collibra.com/blog/data-product-interoperability-native-integration-for-databricks-google-sap-and-snowflake) | Ingests data products built on other platforms as first-class assets | Interoperability with products built anywhere, matching the technology-agnostic stance |

Where they overlap: several of these tools already offer parts of the idea, such as a trust or product score served to AI agents, an MCP server, drift detection and access requests. As far as this brief review found, none combine a tier gradient with a minimum tier per use case, a weakest-input rule across products, caveats returned with the data through every access door, and a separate authority and quality split. That combination is the intended difference, and the checkpoint below tests it properly.

**Build versus adopt.** The main cost of adopting OpenMetadata or DataHub is operational weight, with several services to run, including a search engine. The default is to build a thin catalogue layer on the Bitol open standards, borrowing profiling approaches and metadata models from these projects, and to revisit if the build grows beyond what a small team can maintain. Running one of them purely as a harvesting and profiling engine under our own trust layer stays an option.

### Build-versus-adopt checkpoint

In parallel with the Phase 0 build, run a short, time-boxed spike. Load the flood map and the synthetic sites product into OpenMetadata and into the community edition of Entropy Data, and judge each against the same requirements as this spec.

| Question | What good looks like |
| --- | --- |
| Can it hold a tier, and keep authority and quality separate? | Custom fields or properties that are searchable and returned by its API |
| Can MCP responses carry the tier and caveats with the data? | The server returns them together, or can be extended to |
| Can it enforce a minimum tier and the weakest-input rule? | Configurable, or possible through a small extension |
| How does it handle spatial data? | Geometry types and coordinate system are shown, with OGC API support or a clear path to it |
| How heavy is it to run? | The number of services, the memory needed and the effort to run it in Docker |
| How much control remains over the product page and promotion flow? | Both can be customised, not only themed |
| How much of the commodity work does it cover? | Search, profiling, lineage, access requests and notifications |

The result is reported at the Phase 0 gate and decides one of three outcomes: keep building everything, build the trust layer on top of an adopted catalogue engine, or adopt one tool and extend it.

Source: web searches of current catalogue comparisons, October and November 2026. Not yet reviewed in depth.

## Permissions: who can see a product

Each product is assessed against a simplified version of the ODI Data Spectrum, which runs from closed through shared to open. The result is one access level on the product, with three values. It is called an access level, not a tier, so it is not confused with the trust tiers.

| Access level | Who can see the data | Place on the spectrum |
| --- | --- | --- |
| Open | Everyone with an account on the platform | Internal access. Public access with no sign-in is parked |
| Group | Members of the groups the owner chooses | Group-based access |
| Named | Only the named people, and the owner | Named access |

### Rules

- **Everyone means everyone with an account.** Public access with no sign-in is not offered. Open data such as the flood map is served inside the platform only.
- **Safe by default.** A new product starts as Named, visible to its owner only, until the owner chooses a broader level.
- **One field, not two.** The access level replaces the earlier Public, Internal and Confidential classification. Open data and data suitable for all staff go in Open. Sensitive data uses Group or Named.
- **The same rule on every door.** The product page, SQL, MCP and file export all check the access level, and so will OGC API Features, REST and OData when they arrive. AI use stays a separate flag.
- **Groups are simple at first.** In Phase 1 an admin creates groups and manages their members in the platform, and the owner chooses which groups can see a product. In Phase 3, Entra groups map onto them. Named grants carry an optional expiry date so they do not linger.
- **Restricted products can still be found.** Anyone signed in can find a Group or Named product in search and read its name, description, owner and tier, but not its data. They ask the owner for access. A request workflow follows in Phase 3.
- **The licence sets a ceiling.** A product cannot be given a wider access level than its licence allows, and the platform refuses to save it.
- **Roles are separate.** Consumer, owner, steward and admin say what you can do on the platform. The access level says which data you can see. An owner always has access to their own product.

### How each door enforces it

| Door | How the access level is applied |
| --- | --- |
| Product page | Data preview and downloads shown only to those with access |
| SQL | Read-only database roles, with grants that follow the access level: all users for Open, group roles for Group, and named users for Named |
| MCP | The server acts for the signed-in user, so a query returns only what that person could see elsewhere |
| File export | Checked at the moment of download, with the same row limits as other doors |

## Data rules for the prototype and pilot

These are proposed rules, to confirm with IT and security before the pilot.

- **Public data only at first.** The prototype uses open data, such as the Historic Flood Map under the Open Government Licence, and synthetic data. No BUUK asset, customer or critical-site data is loaded until these rules are agreed.
- **Access level on every product.** The manifest records Open, Group or Named, and it replaces the earlier classification field. The Group and Named levels are tested on synthetic data first, and no sensitive BUUK data is loaded until these rules are agreed.
- **AI use is off by default.** A product is available to AI tools only if its licence permits it and the owner has switched it on. Answers returned through MCP go to the model provider behind the assistant in use, so only public or synthetic data is reachable by AI tools during testing.
- **Licence before ingest.** Every external source needs a licence register entry first. An unknown licence keeps the product at Exploratory only, and attribution wording is kept on every download.
- **No secrets in Git.** Passwords and keys come from environment settings, and stored passwords are hashed. Accounts are named, with no shared logins.
- **Named pilot users.** An admin assigns roles to a short list of people.
- **Audit log kept for 12 months.** It records who did what, but not the contents of query results.
- **Keep the last four versions of each bronze source,** so a bad load can be rolled back.

## Open points and recommendations

The storage engine is decided for the first version: Postgres with PostGIS, with each layer as its own schema. The recommendations below are proposals to confirm or change.

| Open point | Recommendation |
| --- | --- |
| Where Airflow runs | Start on the same host as the app, in separate containers for scheduler, API server and metadata database. Restrict the Airflow UI to admins and keep its metadata database separate from the catalogue's. Use the local executor first and move to a distributed one only when concurrent runs need it. Size the database for concurrent dbt runs, since the transforms run inside Postgres. |
| UI and Git conflicts | Git is the source of truth. The UI saves drafts to a branch and records the base commit. If the base has moved when the user saves, show a diff and ask them to reconcile it rather than auto-merging. Write manifests as YAML with a fixed key order so diffs stay readable. In Phase 1, a per-pipeline edit lock is enough. |
| Failed checks and tier | The manifest marks each check as blocking or warning. A blocking failure (freshness breached, schema break, null keys) downgrades the tier immediately and opens a known issue. A warning only opens an issue for steward review. The tier restores automatically once checks pass, but a Certified product needs the owner to confirm. |
| Owner succession | Require a named deputy alongside the owner, and allow a team alias instead of an individual. Check owners against Entra, and flag a product with no active owner when an account is disabled. If it is not resolved within about 30 days, cap the product below Certified until a steward reassigns it. Add a quarterly owner attestation that the product is still valid. |
| Airflow versioning | Pin one exact release using Airflow's official constraints file, with Cosmos and the dbt adapter pinned alongside it. Upgrade about every six months plus security patches, testing first on a staging copy with sample manifests. Add contract tests for the DAG factory so an upgrade cannot silently change generated DAGs. |

### Still to decide

- **Trust score:** which signals feed it, how they are weighted, and the thresholds that map to each tier.
- **Minimum tier per use case:** whether it is set by the product owner, the consumer, or policy.
- **Caveats in MCP responses:** how they are structured so agents can act on them reliably, not just read them.
- **Spatial performance:** whether the largest spatial transforms run acceptably in Postgres, tested on real datasets before the design is fixed.
- **Restricted data and roles:** whether stewards and admins see Group and Named data by default, and whether a product's existence can ever be hidden from people without access.

### To confirm with others

- **Microsoft admin:** the licences needed to build the Copilot agent, how MCP use is billed (reported as Copilot Studio credits), and what ordinary users need to run a published agent. Sources disagreed on the last point.
- **IT and security:** whether BUUK's approved AI tools may connect to an external MCP server.

### Gaps to design before this is a full platform

- **Identity and access:** Entra single sign-on, roles, and a service identity for MCP and agents.
- **Lineage:** OpenLineage events from Airflow and dbt, so the impact of a bad upstream product is visible.
- **Alerting and subscriptions:** notify owners when checks fail, and consumers when a product changes or is deprecated.
- **Access requests:** a workflow for requesting and approving access to restricted products.
- **Contracts in CI:** align the manifest with the Bitol standards and validate it in CI.
- **Environments and operations:** development, staging and production, secrets, and backups.
- **MCP evaluation:** a test set of questions, like the critical buildings example, to check that AI answers respect tiers and caveats.

### Later features from the reference portals

- AI-drafted column descriptions and other metadata, always confirmed by a person.
- A limitations statement, covering what the data does not tell you, and an assurance statement recording who checked it and when. These could become requirements for Certified.
- Privacy and disclosure controls, recording what was suppressed or generalised in a dataset and its location precision.
- A conforms-to field naming a published standard a dataset follows.
- Style files and guidance documents shipped with downloads.
- Excel and GeoJSON export formats.
- Saved, shareable charts and maps built on products.
- A Python client for the platform.
- A glossary of terms used across products.
- A public roadmap of products and features, published as a dataset with a status for each item.
- Dashboards and a showcase of reuse examples.

### Redrafted sections to review

The product page, known-issues register, licence register, freshness tracking and OData access were rebuilt without the original v0.3 text. Check them against how you intended these to work, in particular the five tab names, the issue severity levels, the Late and Stale tolerance for freshness, and what happens to an unknown licence.
