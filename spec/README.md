# Data Hub (Trusted Data Products): specification pack

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-00 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

**Version:** 1.0, approved at Gate 2 (2026-11-14). **Maturity tier:** 2 for the Phase 0 prototype, 3 from the Phase 1 pilot (D-008). **Deployment:** local Docker Compose. **Scope:** all phases specified (D-007); Phase 0 (Build Brief 001) built first. Readable without the factory: every link is relative, and the IDs are explained in `03` (one line each) and below.

| Document | Purpose |
|---|---|
| [01-overview.md](01-overview.md) | One-page summary |
| [02-product-definition.md](02-product-definition.md) | Objectives, users, journeys, scope |
| [03-functional-requirements.md](03-functional-requirements.md) | What it must do, and its rules |
| [04-user-experience.md](04-user-experience.md) | Screens, navigation and states |
| [05-non-functional-requirements.md](05-non-functional-requirements.md) | Measurable quality attributes |
| [06-data-model.md](06-data-model.md) | Data, relationships, retention |
| [07-security-and-permissions.md](07-security-and-permissions.md) | Access, security and privacy |
| [08-user-stories.md](08-user-stories.md) | Stories and acceptance criteria |
| [09-assumptions-risks-decisions.md](09-assumptions-risks-decisions.md) | Assumptions, questions, risks, dependencies, decisions, changes |
| [10-architecture-and-deployment.md](10-architecture-and-deployment.md) | Components, integrations, deployment |
| [11-delivery-plan.md](11-delivery-plan.md) | Epics, tests, Definition of Done |
| [12-component-selection-and-gaps.md](12-component-selection-and-gaps.md) | Reused assets, gaps and decisions |
| [13-business-process.md](13-business-process.md) | The business process as it is and as it will be, and what changes |
| [traceability.md](traceability.md) | Generated: requirements to stories, design, epics and tests |
| [source-map.md](source-map.md) | Where each section of the three source documents went in this pack |

**IDs:** OBJ objective, JRN journey, FR functional requirement, RULE business rule, SCR screen, NFR non-functional requirement, DATA entity, REL relationship, ROLE, PERM permission, SEC security requirement, US story, AC acceptance criterion, ASM assumption, Q question, RISK, DEP dependency, DEC decision, CR change request, CMP component, INT integration, EPIC, TEST, CAP required capability, CAT catalogue asset.

## Source material
- idea: `idea.md`
- document: `source/v0.5/Trusted Data Products Consolidated Spec (v0.5).md` (v0.5)
- document: `source/v0.5-scope/Trusted Data Products Scope, Phases and Epics.md` (v0.5-scope)
- document: `source/v0.5-brief-001/Prototype Build Brief 001 Historic Flood Map.md` (v0.5-brief-001)

Provenance: the three BUUK documents were imported unchanged on 2026-11-14 (D-001, D-003). The real flood map was inspected read-only (`source/samples/historic-flood-map-schema.md`), and the Airflow, Cosmos, dbt and MCP versions were fixed by an install check in clean containers (`source/samples/install-check-2026-11-14.md`). These paths are relative to the idea folder, outside this pack. The source documents carry no IDs of their own, so `source-map.md` maps them by section.

## Approvals and history
| Date | Version | Event |
|---|---|---|
| 2026-11-14 | 0.1 | Pack created |
| 2026-11-14 | 1.0 | Approved at Gate 2: "decision 1.A", "2B", "3. all ok for now"; CAT-100 to CAT-104 approved and selected |
| 2026-11-16 | 1.1 | CR-001 applied: domain stewards, source and door connectors, API and database sources in Phase 1, secrets store, pilot on a BUUK VM (`../changes.md`) |
