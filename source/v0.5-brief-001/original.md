# Prototype Build Brief 001: Historic Flood Map

Nov 13, 2026 · @Tom Duncan

This brief breaks the Phase 0 prototype into small slices that a coding assistant can build and test one at a time. The prototype takes one real product, the Environment Agency's Historic Flood Map, from download to a published product with a trust tier, a product page, SQL and file access, and an MCP query. It also adds a second, synthetic product so a spatial join across two products can be tested.

The design behind it is in the consolidated spec, *Trusted Data Products: Consolidated Spec (v0.5)*. The scope and epics are in *Trusted Data Products: Scope, Phases and Epics*.

## How to use this brief

Give the assistant one slice at a time, together with the ground rules below. Each slice has a goal, what to build and a **Done when** list that can be checked with tests. Do not start the next slice until the current one's tests pass.

## Ground rules for the coding assistant

- **Postgres with PostGIS only.** All storage is Postgres. Do not introduce other storage engines or file formats for tables.
- **Bronze, silver and gold are schemas** in one database, with a separate catalogue schema for products, runs, checks, issues and the audit log.
- **No AI-generated content.** Descriptions, metadata and text come from rules, harvested source metadata or the owner. Nothing is written by a model.
- **Test first.** Write a failing test, then the code. Use small fixture files for tests, not the full flood map.
- **Do not invent facts about the data.** Inspect the real file's columns and record what you find before writing models that depend on them.
- **Configuration in files.** Thresholds, weights, URLs and schedules live in YAML config, not in code.
- **Everything is logged.** Every run, check, promotion and override writes to the catalogue or the audit log.
- **Small commits,** one slice per branch, and a short note in the repository on what was built and what was left out.
- **Ask when unclear.** If a slice is ambiguous, stop and ask rather than guess.

## Data rules for the prototype

- **Public data only.** Use the flood map, which is under the Open Government Licence, and synthetic data. Never add BUUK asset, customer or critical-site data.
- **Access level on every product.** The manifest records Open, Group or Named, and it defaults to Named. It replaces the earlier classification field.
- **AI use is off by default.** MCP only exposes products where the licence permits AI use and the manifest switches it on. The flood map's manifest switches it on. Anything returned through MCP goes to the model provider behind the assistant, so only public or synthetic data may be reachable.
- **Licence before ingest.** A source without a licence register entry cannot be loaded, and an unknown licence keeps the product at Exploratory only.
- **No secrets in Git.** Passwords and keys come from environment settings, and stored passwords are hashed. Use named accounts only.
- **The audit log** records who did what, never the contents of query results.
- **Keep the last four versions of each bronze source** so a bad load can be rolled back.

## Slices 0 to 3: foundation, manifest, bronze and silver

### Slice 0. Foundation

**Goal:** a repeatable stack that starts with one command.

- A repository with a clear structure, a test runner, linting and a CI workflow.
- A local stack with Postgres and PostGIS, the app, and Airflow, started together.
- A migration tool, and a first migration that creates the `bronze`, `silver`, `gold`, `catalogue` and `audit` schemas.

**Done when:** the stack starts with one command, the tests run with one command, and all five schemas exist after migration.

### Slice 1. Catalogue tables and manifest

**Goal:** a product can be registered from a manifest, and a bad manifest is refused.

- Catalogue tables for products, product versions, licences, known issues, check results, runs, users with roles, groups, group members, named access grants with an optional expiry, and the audit log.
- A YAML manifest format with these required fields: product name, owner, source link, licence, refresh schedule, access level (Open, Group or Named, defaulting to Named), whether AI use is allowed, and the checks with each marked blocking or warning.
- A validator that rejects a manifest with a clear message for each missing or invalid field.
- A licence register entry for the Open Government Licence v3, with the attribution wording shown on the source page, and a sample manifest for the flood map that links to it.

**Done when:** the sample manifest loads and creates a product, each of several invalid manifests is refused with a specific message, and loading is recorded in the audit log.

### Slice 2. Ingest to bronze

**Goal:** the flood map arrives in bronze unchanged, versioned and traceable.

- A task that downloads the GeoPackage from the source link, and also accepts a local file path so tests do not need the network. Keep these as configuration values: the GeoPackage zip at https://environment.data.gov.uk/api/file/download?fileDataSetId=c3e82575-35c8-4ebe-ae39-c6f7b2fcfe40&fileName=Historic\_Flood\_Map.gpkg.zip, and the data version text file at the same address with fileName=Data Version.txt. Read the version file first to decide whether to download.
- Load the file unchanged into a bronze table, with the file hash, load time, source link and source version recorded alongside it.
- Skip the run, and record that it was skipped, when the source version and hash are unchanged.
- Record the file size and load time, because they inform the spatial performance question.

**Done when:** a first run loads the data, a second run with the same file loads nothing and records a skip, and a run with a changed file creates a new version.

### Slice 3. Silver with dbt

**Goal:** cleaned, typed data in the national grid, with every change counted.

- First inspect the real file and write down its columns, types and geometry type in the repository. Build the models from that record, not from assumptions.
- A dbt model that standardises column names and types, confirms the coordinate system is EPSG:27700, repairs invalid geometries and adds a standard key.
- Record how many geometries were repaired, and keep a flag column on each repaired row.
- dbt tests for the key, for required columns and for geometry validity.

**Done when:** the silver table builds from bronze, the dbt tests pass, the repaired count is recorded, and a fixture with a known invalid polygon is repaired and flagged.

## Slices 4 to 7: checks, gold, trust and orchestration

### Slice 4. Checks and freshness

**Goal:** failures are caught, recorded and visible.

- Checks at the bronze-to-silver and silver-to-gold boundaries: row count against the previous run, geometry validity, required columns, and schema drift against a stored baseline.
- Freshness against the quarterly schedule, giving Fresh, Late or Stale, with thresholds in config. Track the created, revised, published and last-run dates separately.
- Each check is marked blocking or warning in the manifest. Results are stored in the check results table.
- A failed blocking check opens a known issue automatically, with the rule, the measured value and the affected scope, and a failed warning opens an issue for steward review only.

**Done when:** a fixture that fails each check produces the right result and the right issue, a passing run produces none, and a stale fixture is marked Stale.

### Slice 5. Profiling and the gold view

**Goal:** a published view that consumers can query safely.

- Profile each column on every refresh: type, range, null rate and distinct count, plus row count, geometry type and coordinate system. Store the results.
- A gold view built by dbt, with quality flag columns so a query can filter out suspect rows.
- Metadata tables, or column comments, that expose the product's tier, freshness and open issues next to the data.
- Read-only database roles scoped to the gold schema, with grants that follow each product's access level: all users for Open, group roles for Group, and named users for Named.

**Done when:** the read-only role can query the gold view and its metadata tables, cannot read bronze, silver or the catalogue, and the profile figures match a hand-checked fixture.

### Slice 6. Trust score, tiers and promotion

**Goal:** a product has a tier, and only a passing product can be promoted.

- A rule-based trust score from authority, freshness, check results, open issues and completeness of required fields, with the weights and thresholds in config.
- A proposed tier from the score, a steward override with a recorded reason, and automatic restore of the tier once blocking checks pass again.
- A promotion step that the pipeline owner performs, allowed only when the checks pass and the manifest is complete. Certified additionally needs a steward sign-off.
- Every score, override and promotion is written to the audit log.

**Done when:** a failing blocking check prevents promotion, a passing product can be promoted by its owner, an override is recorded with its reason, and Certified cannot be reached without steward sign-off.

### Slice 7. DAG factory and embedded Airflow

**Goal:** the pipeline runs from the manifest on a schedule, with no hand-written DAG.

- A DAG factory that reads each manifest and generates the Airflow DAG: ingest, silver, checks, profiling, gold and the promotion-ready state.
- dbt run through Cosmos, so each model is its own task.
- Each task reports its status, duration and log back to the catalogue's run history through a thin interface, so the app never calls Airflow internals.
- The Airflow web interface is not exposed to users.

**Done when:** a flood map run goes from download to gold through the generated DAG, changing the schedule in the manifest changes the DAG, and the run history shows every step.

## Slices 8 to 11: product page, second product, MCP and export

### Slice 8. Product page and sign-in

**Goal:** a person can sign in, find a product and see why to trust it.

- Basic sign-in with four roles: consumer, owner, steward and admin. Entra comes later. An admin creates groups and manages their members, and named grants can carry an expiry date.
- A catalogue list with search and filters for owner, tier, access level and spatial or not. Group and Named products appear with their name, description, owner and tier, but not their data.
- A product page with five tabs: Overview, Data dictionary, Quality, Known issues and Access. The tier, access level, freshness and owner sit in a header above them. Missing fields show as Not set.
- A report-an-issue button that writes to the known-issues register.
- A simple steward list of open findings, and a list of missing fields for owners.

**Done when:** each role sees only what it should, a user outside a product's access level cannot read its data through the product page, SQL, MCP or export, the product page matches the stored data for the flood map, and a reported issue appears in the register and on the Known issues tab.

### Slice 9. Second product and a spatial join

**Goal:** prove that two products can be combined, and that caveats combine too.

- Add a small synthetic product of site polygons, with a network type field, through the same manifest, pipeline and checks. Give it a deliberate, documented known issue so that caveats have something to show. Set its access level to Group, with one test user inside the group and one outside it.
- Run a spatial join across the two gold views, for example flood map areas that intersect the synthetic site polygons for one network type.
- Record the query time and the row counts on the real flood map data.

**Done when:** both products are published, the join returns the expected rows on fixtures for a user in the group and is refused for a user outside it, and the timing on the real data is recorded in the repository.

### Slice 10. MCP server

**Goal:** an AI tool can find, check and query products, and the caveats arrive with the data.

- An MCP server with three tools: search products, describe a product, and query a product with limits.
- Describe returns the schema, tier, freshness, known issues and licence. Query returns the data together with the tier and caveats.
- Queries enforce row limits and the caller's access level, and every call is logged.
- A product below the minimum tier for the request, or marked Not for AI, is refused with a clear reason.
- An answer that draws on more than one product returns the caveats of all of them, and the weakest tier among them.

**Done when:** a scripted agent flow can search, read the tier, run the cross-product query and return the result with combined caveats, a Not for AI product is refused, and the calls appear in the log.

**How to test it, with no Microsoft 365 licence, domain or sign-in service:**

- **MCP Inspector.** Run `npx @modelcontextprotocol/inspector` against the local server, over stdio or at its HTTP address. Use it to call each tool and check the raw responses for the tier, caveats, row limits and refusals.
- **VS Code with GitHub Copilot agent mode.** Add the server with the "MCP: Add Server" command, as stdio or HTTP, then ask questions in agent mode. Check that the assistant carries the caveats into its answer and respects a refusal.
- **A scripted client.** Keep the questions from the demo script as an automated test that asserts on the tier and caveats in each response.

### Slice 11. File export

**Goal:** a download carries its context with it.

- Export a product as CSV, and as GeoPackage for spatial products. No other formats yet.
- Every export includes a small metadata sidecar with the tier, known issues, product version and licence attribution, and a data version file.
- Exports respect the same permissions and row limits as the other doors.

**Done when:** a downloaded file includes its sidecar and version file, the attribution wording matches the licence register, and an export by a user without access is refused.

## Slice 12 and the demo script

### Slice 12. End-to-end demo

**Goal:** one scripted run that shows the whole design working, and records what the prototype learned.

- A demo script that a person can follow in about fifteen minutes, with the expected result at each step.
- A short findings note in the repository covering load times, file size, spatial join timing and any limits reached in Postgres.
- A list of what was left out and what changed from the spec.

**Done when:** the demo script runs start to finish on a clean stack, and the findings note is written.

### Demo script

1. Start from a clean stack and load the sample manifest for the flood map.
2. Run the pipeline from the generated DAG, and watch it reach gold in the run history.
3. Open the product page. The tier, freshness, owner, licence attribution and column profile are shown, with missing descriptions marked Not set.
4. Query the gold view as the read-only role, and read the tier and issues from the metadata tables.
5. Run the cross-product spatial query through MCP, once in MCP Inspector and once in VS Code agent mode, and show the answer arriving with its combined caveats and the weakest tier.
6. Download the product as a file, and show the sidecar and version file.
7. Break something on purpose: load a file with a changed schema. A blocking check fails, a known issue opens, the tier drops, and the MCP response now carries the new caveat.
8. Fix it, rerun, and show the tier restoring and the owner promoting the product again.

### Acceptance for Phase 0

- Every slice's **Done when** list passes.
- The demo script runs without manual fixes.
- The spatial join across the two products ran in an acceptable time on the real flood map data, or the findings note says where Postgres reaches its limit.
- Nothing in the platform produced AI-generated content, and nothing used storage other than Postgres.
- The build-versus-adopt spike has reported with a recommendation. It is run separately from this brief and is not a task for the coding assistant.

### Questions for the first run

- How large is the real flood map file, and how long does it take to load and to process in silver?
- Does the real file's schema match what the spec assumes, and what does the data dictionary need from the owner?
- Which Airflow version and Cosmos release work together for this setup, and are they pinned in the repository?
- Does the embedded Airflow behave well when run on the same host as the app and the database?
