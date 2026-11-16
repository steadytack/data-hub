# Change requests

Changes to anything already approved at a gate (`factory/stages/changes.md`).

| CR | Date | What changed | Reason | Affected IDs | Impact | Approved | Outcome |
|---|---|---|---|---|---|---|---|
| CR-001 | 2026-11-16 | Company-wide data library and wider ingest: domain-scoped ownership, internal data rules and hosting decision; promote API and database pulls from Phase 4 to Phase 1 with a connector interface and secrets handling; add JSON, Excel and Parquet files; adopt `get-started.md` | Spec v0.1 covers one public product plus a small pilot, and ingests only files; the owner asked for a company-wide tool that takes on-prem databases, internal and external APIs and files | FR-015 to FR-017 (reprioritised), new FR-116 to FR-128, EPIC-019 (split), new EPIC-021 and EPIC-022, CMP-004, new CMP-021, INT-019 to INT-022, FR-106, Q-004, Q-001, Q-002, RISK-009, D-007, scope 01 and 02 | Phase 1 grows by about two epics; Phase 0 unchanged; needs an owner decision on hosting and IT and security agreement on credentials | Approved by owner 2026-11-16, with the answers in section F | Approved; applied to the spec 2026-11-16 |

## CR-001 detail (approved 2026-11-16)

Baseline: spec v0.1, Gate 2 approved. The owner approved this change on 2026-11-16. Applied to the spec on 2026-11-16 as FR-116 to FR-128 (FR-128, the source connector interface, was added when applying), EPIC-021 to EPIC-025, CAP-026 and CAP-027, PERM-015 to PERM-017 and TEST-080 to TEST-090. Where this record says RISK-010 to RISK-012 and EPIC-021, EPIC-022 and EPIC-019 as drafted, the spec uses RISK-012 to RISK-014 (010 and 011 already existed) and the epic split shown in doc 11. Where an ID is shown as "new", the number is provisional and is fixed on approval.

### A. Company-wide data library

| Gap found | Change |
|---|---|
| No domain-level ownership: one steward role for everything | **FR-116** (Must, Phase 1): each `domain` has a named domain steward and a deputy; a steward triages, overrides and signs off only products in their domains; the platform admin manages domains. Change RULE-008 and PERM matrix to add "domain steward" without granting data access (ASM-005 still holds) |
| Catalogue does not scale beyond one team | **FR-117** (Should, Phase 2): domain pages and a browse-by-domain view with product counts, tier mix and stale products per domain |
| Internal data blocked until data rules are agreed | **Q-001 closes as a Phase 0 gate deliverable:** a one-page data rules note agreed with IT and security covering classification (internal, confidential), who may be an owner, retention, and that `internal` data is never AI-enabled (RULE-001 stays). Needed before any internal product enters the pilot |
| Pilot products and owners unnamed | **Q-001 and Q-002:** name three to five internal products across at least two domains and their owners and stewards, so the pilot proves the multi-team model rather than one team's use |
| Single host, no company hosting plan | **Decision DEC-016 (owner to choose):** where Phase 1 runs for pilot users. Options: (a) a BUUK-hosted VM on the internal network, recommended because internal sources and on-prem databases are reachable and data stays inside; (b) a cloud tenant; (c) keep the laptop (not viable beyond a demo). Phase 1 pulls the environments work (FR-106) forward if (a) or (b) |
| Onboarding is undocumented | **FR-118** (Must, Phase 1): `get-started.md` (this change adds it) becomes the owner's guide, linked from the app; its examples are replaced with the real flood map manifest and the first real internal manifest once they exist (RISK-009) |

### B. Ingest from any source

Design rule (**DEC-017**): every source type is a *connector* with one interface: `check_version()`, `fetch()` returning files or batches, and `describe()` returning the schema. Bronze, silver, checks and everything after stay unchanged. New CMP-021 (connector library) in CMP-004's package. The first connectors are existing download and upload code refactored behind it (no behaviour change; TEST-007, TEST-008 must still pass).

| New or changed requirement | Phase | Notes |
|---|---|---|
| **FR-015** API polling: raise Could/4 to Must/1; generalise to "an HTTP API", with paging, a staleness threshold and a record-count check | 1 | Covers internal and external REST APIs |
| **FR-119** API authentication: API key in header, bearer token and OAuth2 client credentials; paging styles page/offset/cursor/Link header; configurable in the manifest, never the secret itself | 1 | Internal APIs |
| **FR-017** external database pull: raise Could/4 to Must/1; Postgres and SQL Server first (Oracle later as a further connector), via read-only credentials, a SQL query or table and an incremental key | 1 | On-prem and cloud-hosted databases |
| **FR-120** private-network reach: a runner on the source's network (an Airflow worker or a small agent) pulls the data and pushes it to bronze over one outbound HTTPS connection; the hub never opens inbound access to a source network | 4 (held, owner reply 5) | Built only when a named source cannot be reached from the hub's VM (DEC-016) |
| **FR-121** secrets store: connection secrets held in a secrets backend (Docker secrets in Phase 1, the company vault later), referenced by name in the manifest, never logged or written to audit, never returned by the API | 1 | New **SEC-017** and **PERM** rows; TEST for no secret in logs or manifests; gitleaks already in CI (TEST-110) |
| **FR-122** file formats: add JSON/JSONL, Excel (.xlsx) and Parquet to upload and download; file type and size limits as FR-013 and SEC-013 | 1 | Standard library and light parsers only; dependency choices go through the catalogue (POL-10) |
| **FR-123** a connection test: "test source" in the manifest check reports reachable, authenticated, first 10 rows and detected schema before the first run | 1 | Owner self-service |
| **FR-124** sensitive-column handling: a manifest can mark columns `sensitive`; they are excluded from gold and previews unless the product is Named | 2 | Internal data with personal fields |
| **FR-016** OGC API Features ingest: stays Could, Phase 4 | 4 | Not needed for the stated sources |
| **FR-125** change data capture, streaming and queue sources | 4 (parked) | Recorded so it is a conscious exclusion |

Postgres remains the only store (NFR-012); connectors write to bronze only.

### C. Epics, components and integrations

- **EPIC-019** split: *Connector framework and file formats* (FR-119, FR-122, refactor of FR-009/010/013) and *API and database sources* (FR-015, FR-017, FR-119 to FR-121, FR-123), both Phase 1, after EPIC-011.
- **EPIC-021** (new, Phase 1): domains and stewardship (FR-116, FR-118).
- **EPIC-022** (new, Phase 2): domain browse and sensitive columns (FR-117, FR-124).
- **CMP-021** connector library and runner; **INT-019** internal API source, **INT-020** on-prem database source, **INT-021** external API source, **INT-022** secrets backend. Each needs a saved sample before its epic starts (RISK-009).
- New risks: **RISK-012** source credentials leak (high impact; mitigation FR-121, read-only accounts, rotation); **RISK-013** pulls load a production database (mitigation: replicas or off-peak schedules, row and time limits per pull); **RISK-014** no network route to a source (Q-004).
- New tests: one per new FR, using fixture API and database containers and never real sources; add a "no secret in any log" check.

### D. Phase and gate effect

Phase 0 and its gate are unchanged. Phase 1 gate adds: one internal API and one on-prem database product live in the pilot, owned by two different domains, with no secret visible anywhere. Tier 3 applies as already agreed (D-008).

### E. Door connectors (added after the owner's reply)

The same pattern applies on the consumer side. Today the doors (product page, SQL, MCP, export, and later OGC API, REST, OData) are separate pieces of code that each call the access service. Make a door a **connector** too (**DEC-018**), so doors can be added over time without touching the platform.

| New or changed requirement | Phase | Notes |
|---|---|---|
| **FR-126** door connector interface: `describe()` (what the door offers for a product), `serve(request, identity)` (always through the one access service, FR-071), and a declaration of what it returns (data, tier, caveats, licence attribution). Row limits (RULE-011), rate limits (FR-075) and usage logging (FR-076) are applied by the platform around every door, not by each door | 1 | Refactor of the Phase 0 doors with no behaviour change; TEST-029, TEST-039, TEST-042 and TEST-113 must still pass unchanged |
| **FR-127** a door registry: an admin sees which doors are enabled and can switch one off per product or globally; a door a product's licence or `ai_use` forbids is never offered (as FR-099 already requires for OData) | 1 | Gives one place to prove "Not for AI" and "internal only" hold on every door |
| Existing door requirements (FR-094 to FR-099, FR-082 to FR-087) | unchanged | Each becomes an implementation of FR-126 |

New component **CMP-022** (door connector library, in CMP-001's package) and a short guide for writing a connector, one for sources and one for doors.

### F. Decisions (owner reply, 2026-11-16)

| # | Decision | Recorded as |
|---|---|---|
| 1 | Pilot runs on a BUUK-hosted VM or server | **DEC-016** accepted. Phase 1 environments (FR-106) are pulled forward to match |
| 2 | FR-015 and FR-017 move to Phase 1, built in stages: connector framework and secrets store, then the API connector, then the database connector | Approved for this change; **D-007** amended for these two items |
| 3 | The hub's own store is Postgres. Source connectors for **Postgres and SQL Server** first. **Oracle** is not first: it is added later as a further connector, not a design change | **DEC-019**; FR-017 scope names Postgres and SQL Server |
| 4 | The data rules note is a Phase 0 gate deliverable; pilot owners come from the data's own teams | Q-001 and Q-002 stay open until named |
| 5 | Sources sit on different VMs and some are hosted in Azure and AWS. The connector runner (FR-120) is **held**, not built, unless a source cannot be reached from the hub's VM | FR-120 moved to Phase 4 (parked) with a trigger: a named source with no network path |

**Consequence of 5 that needs checking (new open question Q-004):** with sources on other VMs and cloud accounts, the hub's VM must be able to reach each one. IT and security need to confirm, per source, the route (network or VPN, private endpoint, or an allow-listed public address), the read-only account, and who rotates the credential. Without a route a source cannot be a pilot product. Choose pilot sources that are already reachable from the VM's network first.

### Remaining open points

- Q-001 and Q-002: name the pilot products, owners and domain stewards.
- Q-004: network route and read-only account for each pilot source.
- Oracle: add a connector when a pilot or later product needs one.
- The approval of CR-001 itself, once you are content with it as a whole (the Approved column stays "Draft" until then).
