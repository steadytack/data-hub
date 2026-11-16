# Data Hub (Trusted Data Products)

A platform for publishing, finding and trusting data products across BUUK. An owner describes a dataset in a YAML manifest; the platform ingests it, cleans it, checks it, scores how far it can be trusted, and serves it with its quality caveats through a catalogue page, SQL, MCP (for AI tools) and file export.

**Status:** specified, not yet built. The spec (v1.1) is approved. Phase 0, a prototype using the Environment Agency Historic Flood Map, is the first build.

## Where to start

| If you want to | Read |
|---|---|
| Put a dataset into the Data Hub | [get-started.md](get-started.md): manifests for a public file, a database, an API and an upload |
| Understand the idea in one page | [spec/01-overview.md](spec/01-overview.md) |
| See what it must do | [spec/03-functional-requirements.md](spec/03-functional-requirements.md) (FR-001 to FR-128, with business rules) |
| See how it is built | [spec/10-architecture-and-deployment.md](spec/10-architecture-and-deployment.md) and [spec/11-delivery-plan.md](spec/11-delivery-plan.md) (epics and tests) |
| See the open decisions, risks and questions | [spec/09-assumptions-risks-decisions.md](spec/09-assumptions-risks-decisions.md) |
| See what changed after the spec was approved | [changes.md](changes.md) (CR-001: company-wide use and wider ingest) |

The full spec pack is in [spec/](spec/README.md), which lists every document and explains the IDs (FR, RULE, US, TEST and so on). [spec/traceability.md](spec/traceability.md) links each requirement to its stories, rules, epics and tests.

## How it works

1. An owner registers a **manifest**: owner, source, licence, domain, schedule, access level and checks.
2. The platform generates a pipeline: **bronze** (raw copy) → **silver** (cleaned) → **gold** (published view).
3. Checks, profiling, freshness and a trust score run automatically and propose a tier: Exploratory only, Usable with caveats or Certified. Certified needs a domain steward's sign-off.
4. Consumers reach the product through **doors**, each applying the same access rule and returning the tier and caveats with the data.

Every **source** (file, upload, HTTP API, Postgres or SQL Server database) and every **door** is a connector behind one interface, so both can grow over time.

## What is in this repository

| Path | Contents |
|---|---|
| `idea.md`, `overview.md`, `application-definition.md` | The idea and what was agreed at the first gate |
| `spec/` | The specification pack (v1.1) |
| `source/` | The three BUUK source documents (v0.5 spec, scope, Build Brief 001), imported unchanged, plus samples and the install check |
| `changes.md` | Change requests after approval |
| `decisions.md`, `gates/` | Owner decisions and the gate approvals |
| `state.json`, `composition.json` | Machine-readable status and the planned components |

## Phases

| Phase | What it delivers |
|---|---|
| 0 | Prototype: flood map, trust tiers, product page, SQL, MCP, export |
| 1 | Internal pilot on a BUUK-hosted VM: domain stewards, API and database sources, uploads, secrets store, door connectors, separate environments |
| 2 | More doors (OGC API Features, REST, OData), pipeline builder, notifications |
| 3 | BUUK-wide: Entra sign-on, Copilot agent, governance, lineage |
| 4 | Parked: OGC API ingest, source-network runner, change-data capture, other later features |

## Open items

- Name the pilot products, owners and domain stewards (Q-001, Q-002).
- Confirm the network route and read-only account for each pilot source, some of which sit on other VMs or in Azure or AWS (Q-004).
- Agree the data rules note with IT and security before any internal data is used.
- Request the BUUK VM for the pilot (DEC-016).

## Notes for readers

- This folder was produced by a software factory process. Some files refer to factory paths (such as `factory/context/defaults.md`) and to catalogue assets (`CAT-nnn`) that are not in this repository; the spec pack stands without them.
- The manifest examples in `get-started.md` are illustrative until the first real manifests exist.
- The source documents belong to BUUK; keep this repository private.
