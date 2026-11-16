# Data Hub: get started

How to put a dataset into the Data Hub and have it appear as a trusted data product. Based on spec v1.1 (CR-001 applied): `spec/03-functional-requirements.md`, RULE-001.

**Status of what is described.** Phase 0 (the flood map, file download) is built first. Uploads, API sources, database sources, secrets, the source test, domain stewards and door switches are Phase 1 and are not built yet. The manifests below are **illustrative**: the required keys come from RULE-001, but the spec has no finished sample manifest, so optional keys (abstract, contact, dictionary) and the exact `connector` keys are settled when the connector epics (EPIC-019, EPIC-023) are built. FR-118 keeps this guide in step with the validator.

## What you do, in one page

1. **Make a connection** (API and database sources only): an admin or owner registers a named connection (type, address, read-only account, and the *name* of its secret). The secret itself goes in the secrets store, never in a manifest (FR-121).
2. **Test the source** before the first run: the platform reports reachable, authenticated, the first 10 rows and the detected schema, and stores no rows (FR-123).
3. **Write a manifest** (YAML): owner, source, licence, domain, schedule, access level, AI-use flag, data origin and checks.
4. Save it in the pipelines repository. The platform validates it and refuses it with one message per bad field (FR-003).
5. The platform generates the pipeline (one Airflow DAG per manifest, FR-038): **bronze** (raw copy) → **silver** (cleaned) → **gold** (published view).
6. Checks, profiling, freshness and a trust score run automatically and propose a tier: Exploratory only, Usable with caveats or Certified.
7. The owner fills in what the platform cannot know (definitions, abstract, contact), then promotes silver to gold (FR-032). A Certified tier needs the sign-off of the **domain steward**, who cannot be the product's own owner (FR-033, FR-116).
8. People find it in the catalogue (browse by domain from Phase 2) and use it through any enabled **door**: product page, SQL, MCP (AI tools) or export. Admins can switch a door off globally or per product (FR-127).

You never write a DAG or hand-load a table.

## Rules every manifest follows (RULE-001)

| Key | Meaning |
|---|---|
| `name`, `title` | Unique slug and display name |
| `owner` | An existing user with the owner role |
| `source` | `link` (+ optional `version_link`), `upload`, or a `connector` with its `connection` name and a `query` or `endpoint`. A literal secret is refused |
| `licence` | An ID from the licence register. **No registered licence, no ingest** (FR-005) |
| `schedule`, `expected_cadence` | Cron for runs; days between expected updates (drives freshness) |
| `access_level` | `Open`, `Group` or `Named`. Absent means Named. A licence can cap how wide it may be (RULE-012) |
| `ai_use` | `true` only if `data_origin` is `public` or `synthetic`. Absent means false |
| `data_origin` | `public`, `synthetic` or `internal` |
| `domain` | Catalogue grouping; its steward signs off Certified |
| `checks` | Each has `rule`, `boundary` (bronze-silver or silver-gold) and `severity` (blocking or warning) |

Unknown keys are refused by name.

## Choosing a source

Every source type is a connector with the same interface (check version, fetch, describe), so everything after bronze is identical (FR-128).

| Source | Phase | Use it for |
|---|---|---|
| Public file download with a version file | 0 | Open data such as the flood map |
| Local file path | 0 | Development |
| Upload: CSV, GeoPackage, JSON or JSON Lines, Excel, Parquet | 1 | Files teams already hold; exports from systems you cannot connect to |
| HTTP API, internal or external | 1 | Fast-refresh data; API key, bearer token or OAuth2 client credentials; page, offset, cursor or Link-header paging |
| Postgres or SQL Server database, on-prem or in Azure or AWS | 1 | Tables or queries, with an optional incremental key |
| Oracle | Later | Added as a further connector when a product needs it |
| OGC API Features, change-data-capture or streams, a runner inside a source network | Parked (Phase 4) | Only when a need is confirmed (FR-016, FR-125, FR-120) |

## Example 1: external source (a public download)

The Phase 0 product, the Environment Agency Historic Flood Map.

```yaml
name: historic-flood-map
title: Historic Flood Map
owner: data.owner@example.org     # an existing owner-role user
source:
  link: https://<EA download URL>/Historic_Flood_Map.gpkg.zip
  version_link: https://<EA download URL>/data-version.txt
licence: OGL-v3                    # register this first (FR-004)
schedule: "0 6 * * 1"              # weekly version check (FR-043)
expected_cadence: 90               # quarterly source
access_level: Open
ai_use: true                       # allowed: data_origin is public
data_origin: public
domain: environment
checks:
  - {rule: row_count_vs_previous, boundary: bronze-silver, severity: blocking}
  - {rule: geometry_valid,        boundary: bronze-silver, severity: warning}
  - {rule: required_columns,      boundary: bronze-silver, severity: blocking}
  - {rule: schema_drift,          boundary: silver-gold,   severity: blocking}
```

Each week the platform reads the version file. If the text and file hash are unchanged it records a skipped run (RULE-003). If changed, it downloads, lands the file unchanged in bronze (last four versions kept), builds silver, runs checks and refreshes the score. Freshness is read from "This is the September 2026 release."

Steps: add the OGL v3 licence entry; save the manifest; start the stack (`docker compose up -d`) and trigger the first run; check the product page; fill the dictionary; promote; try SQL, MCP or export from the Access tab.

## Example 2: internal database (on-prem or cloud, Postgres or SQL Server)

Phase 1. Typical case: a site asset register in a SQL Server database on another VM.

**Before the manifest**
1. Ask IT and security for a **read-only account** on the source and confirm the **network route** from the hub's VM (network or VPN, private endpoint, or an allow-listed address), and who rotates the credential (Q-004). Choose sources already reachable first. Where possible point at a read replica, and schedule off-peak (RISK-013).
2. Put the password in the secrets store under a name, for example `sqlserver_assets_ro`.
3. Register the connection (name `assets-db`, type `sqlserver`, address, account name, secret reference `sqlserver_assets_ro`).
4. Create the `INTERNAL-ONLY` licence entry with a Group ceiling, so `access_level: Open` is refused.
5. Run **test source**. Fix any "unreachable" or "authentication failed" result before going on.

```yaml
name: site-asset-register
title: Site Asset Register
owner: asset.owner@example.org
source:
  connector: sqlserver
  connection: assets-db            # a name; the secret is never in the manifest
  query: "SELECT AssetId, SiteId, AssetType, InstalledOn, ModifiedOn FROM dbo.Assets"
  incremental_key: ModifiedOn      # later runs add only changed rows
licence: INTERNAL-ONLY
schedule: "0 2 * * *"              # nightly, off-peak
expected_cadence: 1
access_level: Group                # groups chosen on the product
ai_use: false                      # required: data_origin is internal
data_origin: internal
domain: assets
checks:
  - {rule: required_columns, boundary: bronze-silver, severity: blocking}
  - {rule: row_count_vs_previous, boundary: bronze-silver, severity: warning}
  - {rule: schema_drift, boundary: silver-gold, severity: blocking}
```

After saving: create the group that may read it, complete the dictionary, promote. Because `ai_use` is false the product is **Not for AI** and the MCP door refuses it; SQL, the product page and export still work for group members. Mark personal columns `sensitive` once that arrives (FR-124, Phase 2), and avoid personal data until then.

For Postgres use `connector: postgres`. If the source cannot be reached from the hub's VM at all, the fallback is a CSV or Excel upload (Example 3) until a network route exists.

## Example 3: internal API

Phase 1. A REST API that returns JSON pages.

```yaml
name: permit-register
title: Permit Register
owner: permits.owner@example.org
source:
  connector: http_api
  connection: permits-api          # holds address and auth type; secret by name
  endpoint: /v2/permits
  paging: cursor                   # page, offset, cursor or link_header
licence: INTERNAL-ONLY
schedule: "*/15 * * * *"           # as short as every 15 minutes (FR-015)
expected_cadence: 1
access_level: Group
ai_use: false
data_origin: internal
domain: operations
checks:
  - {rule: required_columns, boundary: bronze-silver, severity: blocking}
  - {rule: record_count_vs_previous, boundary: bronze-silver, severity: warning}
```

The pages land in bronze unchanged with the version and record count. External APIs work the same way with `data_origin: public` or a third-party licence. Respect the provider's terms and rate limits; the platform backs off and retries on rate-limit replies.

## Example 4: a file you cannot connect to

Export a CSV, Excel, JSON Lines or Parquet file, then use `source: {upload: true}` and upload it on the product page. It lands in bronze exactly as a download does. A new upload creates a new version, and the previous four are kept for rollback.

## Roles in one line each

- **Owner:** registers manifests, fills missing fields, sets access, promotes. Cannot sign off their own Certified.
- **Domain steward (and deputy):** triages issues, overrides tiers with a reason, signs off Certified, only for their domain.
- **Admin:** manages users, groups, domains, connections, doors and backups.
- **Consumer:** finds products, reads where the access rule allows, reports issues.

A role never grants data access by itself; access follows the product's level (RULE-008).

## Limits to keep in mind

- Phase 1 is for a small pilot on a BUUK-hosted VM. The hub's VM needs a network route to every source.
- Oracle is not supported yet.
- Internal data is never AI-enabled. Internal data also waits for the data rules note agreed with IT and security (a Phase 0 gate deliverable).
- Row limits apply on the doors (for example 1,000 rows per MCP call by default).
- Examples above are drafts until the first real manifests exist (RISK-009); then this guide is updated from them.
