# Gate 2: Data Hub (Trusted Data Products) (spec pack, composition, delivery plan)

Status: approved 2026-11-14. Date: 2026-11-14. Spec pack 1.0, composition 1.0 (validation `not-run`), against application definition 1.0.

## Approval
Reply (2026-11-14): "decision 1.A, i cant decide with 2 ... but what about if the user using ai has access to more of the data products? 3. all ok for now", then, after the options were explained, "2B". All three as recommended.
- Decision 1.A (D-017): `catalogue.py validate --yes` passed for CAT-100 to CAT-104 (static review; their own test suites not run); `catalogue.py approve --by owner` for all five. Doc 10 (CMP-005, CMP-006, CMP-008), doc 12 (CAP-003 to CAP-006 now reuse) and `composition.json` switched to them; DEP-002 closed.
- Decision 2.B (D-018, DEC-007): over MCP, any product with AI use ticked is reachable, never beyond what the calling user can read on any other door; AI use can be switched on only for public or synthetic data in Phases 0 and 1.
- Decision 3 (D-019): DEC-005, DEC-006, DEC-008, DEC-009 and ASM-013 confirmed "for now".
- Checks after the changes, run 2026-11-14: `check_spec.py data-hub` OK, 0 blockers, 6 warnings (the Phase 2 and 3 external components CMP-011, CMP-014 to CMP-018); `--analyze` OK, 0 critical, 0 high, 0 medium, 14 low; `check_factory.py` OK; `catalogue.py validate` 104 entries, 0 problems. No learned.md entry: every answer matched the recommendation.

## Done
- **Spec pack** `spec/` (README, 01 to 13, `source-map.md`): all phases in full (D-007). 9 objectives, 14 journeys, 115 FRs (Phase 0: 64 and Phase 1: 15, Must; Phase 2: 14 and Phase 3: 13, Should; Phase 4: 9, Could), 16 rules, 18 screens, 15 NFRs, 28 entities, 6 roles, 14 permissions, 16 SEC, 79 stories with 158 acceptance criteria, 15 decisions, 20 components, 18 integrations, 25 capabilities, 20 epics, 99 tests. Every source section mapped or deferred (the sources carry no IDs).
- **To-be process (doc 13):** an owner writes a manifest once; the platform ingests, checks, profiles, scores and documents the product on schedule; the owner fills gaps and promotes, a steward signs off Certified; consumers find it and query by SQL, MCP or download with the tier and caveats attached; reported issues and failed checks feed back into the tier.
- **Catalogue (D-009):** Airflow, Cosmos, dbt-core, dbt-postgres and the MCP Python SDK imported from their PyPI wheels as **CAT-100 to CAT-104** (SRC-028 to SRC-032), statically assessed, 0 blocker findings. CAT-100's metadata was set by hand from its wheel (the importer read nothing from the metadata-only Airflow wheel).
- **Install check, run in this session in clean `python:3.12-slim` containers** (`source/samples/install-check-2026-11-14.md`):
  - All four pipeline libraries in one environment with Airflow's constraints: **failed** (dbt-core 1.12.7 needs `pathspec<1.1`).
  - Airflow 3.4.0 + Cosmos 1.16.0 with the constraints file: **passed** (`pip check` clean, imports work).
  - dbt-core 1.12.7 + dbt-postgres 1.11.0 in their own virtual environment: **passed**.
  - Web-app backend requirements + mcp 2.3.0: **passed**.
  - So dbt runs from its own virtual environment in the Airflow image (DEC-004).
- **`composition.json`:** one component on CAT-006 1.4.0 (web-app, adapted) plus CAT-056 gitleaks; eight links; validation `not-run`; the five libraries listed as pending assets until approved.
- **Delivery plan (doc 11):** Phase 0 = EPIC-001 to EPIC-009 (brief slices 0 to 12), Phase 1 = EPIC-010, EPIC-011 (Tier 3), Phase 2 = EPIC-012 to EPIC-015, Phase 3 = EPIC-016 to EPIC-018, Phase 4 parked = EPIC-019, EPIC-020.
- **Checks, run 2026-11-14 in this session:**
  - `python factory/scripts/check_spec.py data-hub`: **OK, 0 blockers, 9 warnings**; wrote `spec/traceability.md`.
  - `python factory/scripts/check_spec.py data-hub --analyze --report spec/analysis.md`: **OK, 0 critical, 0 high, 0 medium, 14 low**; wrote `spec/analysis.md`.
  - Not run: `catalogue.py validate` for CAT-100 to CAT-104 (refused by this session's permission check because it can run imported code); `check_factory.py`; any build or stack.

### Warnings (9), each accepted with a reason
- CMP-005, CMP-006, CMP-008 use dbt, Airflow with Cosmos, and the MCP SDK, which are not yet approved: cleared by decision 1 below.
- CMP-011 and CMP-014 to CMP-018 are Phase 2 and 3 components (OGC API server, mail relay, Entra, reverse proxy, Copilot agent, lineage store), each chosen and approved before its epic (CAP-016 to CAP-023).

### LOW findings (14)
TEST-101, TEST-104 to TEST-107, TEST-109 to TEST-111, TEST-113, TEST-115, TEST-116, TEST-118 to TEST-120 verify NFR, SEC or PERM items rather than an AC or FR, by design.

## Definition of Done
| Item | State |
|---|---|
| Definition approved | done (Gate 1, 1.0) |
| Users, outcomes and scope clear | done (02) |
| Tier, deployment, interfaces recorded | done (01, 07, 10) |
| Design decisions recorded | done (09, DEC-001 to DEC-015) |
| Requirements testable for the tier | done (03; every FR has a verification) |
| NFRs defined | done (05, thresholds and tests) |
| Screens and journeys documented | done (04, 18 screens and record tables) |
| As-is and to-be process and what changes, every journey covered | done (13) |
| Glossary | done (01; no acronym warnings) |
| Data model at the right level | done (06) |
| Security and privacy proportionate | done (07: tier 2 for Phase 0, tier 3 from Phase 1) |
| Stories have acceptance criteria | done (08, 158 criteria) |
| Assumptions, risks, decisions, questions recorded | done (09); Q-001 and Q-002 open, block release of Phase 1, not the spec |
| Catalogue discovery done | done (12, 2026-11-14) |
| Doc 12 complete | done (25 capabilities, each with a decision) |
| `composition.json` created | done |
| Compatibility and licence as far as verified | done: install check; Apache-2.0 (Airflow, Cosmos, dbt-core, dbt-postgres), MIT (mcp) |
| Every gap has a decision | done |
| Architecture consistent with the composition | done |
| Plan has epics, stories, dependencies, release boundaries | done (11) |
| Traceability holds | done (`traceability.md`) |
| `check_spec.py` no blockers | done (0) |
| Pack has its index and version | done (README, 0.1) |

## Recommended
- **First release:** Phase 0 as EPIC-001 to EPIC-009, one brief slice at a time, tests first; Tier 2. Raise to Tier 3 at EPIC-010 (D-008).
- **Build order:** EPIC-001 foundation → EPIC-002 catalogue and manifest → EPIC-003 bronze and silver → EPIC-004 checks, gold and trust → EPIC-005 DAG factory → EPIC-006 sign-in and product page → EPIC-007 second product and join → EPIC-008 MCP and export → EPIC-009 demo and findings (Phase 0 gate).
- **Composition:** web-app CAT-006 1.4.0 adapted; Airflow 3.4.0 + Cosmos 1.16.0 under Airflow's constraints, dbt 1.12.7 + dbt-postgres 1.11.0 in their own environment in the Airflow image; MCP SDK 2.3.0 in the backend image; everything else custom (manifest validator, DAG factory, checks and scoring, access service, GeoPackage module, MCP tools).

## Decisions for you
1. **Approve the five imported libraries** CAT-100 apache-airflow 3.4.0, CAT-101 astronomer-cosmos 1.16.0, CAT-102 dbt-core 1.12.7, CAT-103 dbt-postgres 1.11.0, CAT-104 mcp 2.3.0 (POL-10)?
   - **A (recommended):** approve all five; the factory then runs `catalogue.py validate` and `approve --by owner` and switches doc 10, doc 12 and `composition.json` to them. Because the install check passed and the source requires them.
   - B: approve Airflow, Cosmos and MCP now; hold dbt until EPIC-003 proves it in a container.
   - C: approve none yet; Build stops at EPIC-001.
2. **MCP reach for the synthetic Group product** (DEC-007): the definition said only Open products reach AI tools; the brief's slices 9 and 10 need the Group-level synthetic product in the MCP join.
   - **A (recommended):** AI use allowed for public or synthetic data at any access level, still subject to access. Because it follows the brief's data rule.
   - B: only Open products over MCP; the cross-product MCP join uses an Open synthetic product instead, and the Group refusal is tested on SQL and export only.
3. **Factory calls to confirm** (reply with any you want changed): DEC-006 a steward override cannot lift an open blocking issue's cap; DEC-008 Not for AI shown as the tier; DEC-009 MCP query is one read-only SELECT; DEC-005 GeoPackage via the standard library; ASM-013 the flood map's authority is "reference copy" (score 91, Usable with caveats until sign-off).

## Answered from your defaults
Deployment local Compose (DEF-02), ports 127.0.0.1 (POL-06); named accounts with roles (tier 3 row, DEF-03 extended); PostgreSQL with PostGIS (DEF-04); backups nightly (DEF-06); UK locale (DEF-07); accessibility (DEF-08); unbranded, quiet UI (DEF-09); a test per requirement (DEF-10); free services first, so notifications wait for a mail service (DEF-11); no offline (DEF-12); MIT, to be confirmed at Publish as BUUK work (DEF-13); private repository (DEF-14); template stack only (DEF-15); milestones of epics (DEF-16); template features with Share off and feedback stored (D-006). Factory decisions D-011 to D-016.

## Risks
- RISK-010: until decision 1, Build cannot use Airflow, Cosmos, dbt or the MCP SDK.
- RISK-002: the libraries are proven as installs, not yet as a DAG run (EPIC-005's first test).
- RISK-007: spatial join speed on 21,578 multipolygons unmeasured until EPIC-007.
- RISK-004: MCP data reaches the model provider; limited to AI-enabled public or synthetic products (decision 2).
- RISK-009: no samples yet for the EA metadata record or the Phase 2 and 3 integrations.
- RISK-008: dbt v2 is now announced as the default; the pack stays on 1.x.

## Next
On `approve` (or `change N to X`): record the reply here, in `state.json` and `decisions.md`; run `catalogue.py validate` and `approve --by owner` for the chosen assets and switch doc 10, doc 12 and `composition.json` to them; then Build (`factory/stages/build.md`) in a fresh session: `/factory resume data-hub`, starting with EPIC-001.

Reply with `approve`, `accept all`, `change N to X`, or `back to analyse`.
