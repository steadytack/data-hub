# Source map

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-SM | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

The three source documents carry no IDs of their own (`source/*/ids.json` is empty), so this maps them by section. `v0.5` is *Trusted Data Products: Consolidated Spec (v0.5)*, `scope` is *Scope, Phases and Epics*, `brief` is *Prototype Build Brief 001: Historic Flood Map*. Status is Mapped or Deferred (with the reason).

| Source section | Maps to | Status | Note |
|---|---|---|---|
| v0.5 Why a data product, not a database connection | OBJ-002, OBJ-003, 13 As-is | Mapped | The five reasons become objectives |
| v0.5 Trust is a gradient; Trust tiers | FR-028, FR-029, RULE-005, RULE-006, DEC-008 | Mapped | |
| v0.5 Combined answers; What Certified requires | FR-084, RULE-007, FR-033 | Mapped | |
| v0.5 The publish contract | FR-002, FR-003, RULE-001 | Mapped | |
| v0.5 What the platform builds itself | FR-025, FR-027, FR-052 to FR-057 | Mapped | |
| v0.5 Harvest first; Change over time | FR-027, FR-026, RULE-015 | Mapped | |
| v0.5 Automation with a human in the loop | FR-034 to FR-037, FR-061, NFR-011 | Mapped | |
| v0.5 The product page | FR-052 to FR-057, SCR-003 | Mapped | Redrafted in the source; five tab names kept as given |
| v0.5 Known-issues register | FR-006, FR-007, RULE-014 | Mapped | Severity levels minor, material, blocking kept |
| v0.5 Licence register | FR-004, FR-005, FR-070, RULE-012 | Mapped | Unknown licence holds at Exploratory only |
| v0.5 Freshness tracking | FR-023, FR-024, RULE-004 | Mapped | Late and Stale tolerances from ASM-007 |
| v0.5 Requests, feedback and trying the API | FR-062, FR-063, FR-064 | Mapped | Phase 2, as the scope document places them |
| v0.5 Pipelines: Decisions made | FR-032, FR-038, CMP-006, DEC-001 | Mapped | |
| v0.5 Medallion layers and trust tiers | FR-001, FR-018, FR-019, RULE-006 | Mapped | |
| v0.5 Pipeline manifest and UI builder | FR-002, FR-045 to FR-048 | Mapped | |
| v0.5 Pipeline builder UI | FR-045 to FR-049, SCR-015 | Mapped | Editable canvas deferred to FR-113 |
| v0.5 Embedded Airflow | FR-038 to FR-041, FR-044, SEC-003 | Mapped | |
| v0.5 Quality gates and promotion to gold | FR-022, FR-032, RULE-009 | Mapped | |
| v0.5 Reporting run results to the catalogue | FR-041, FR-042, FR-107 | Mapped | |
| v0.5 Access: doors table | FR-071, FR-082, FR-094, FR-097, FR-098, FR-099 | Mapped | Arrow Flight, Python SDK, WMS and WFS: FR-115 |
| v0.5 How this shows up in the MCP layer | FR-082 to FR-088 | Mapped | |
| v0.5 SQL access for traditional tools | FR-072, FR-020, SEC-004 | Mapped | |
| v0.5 OData access | FR-099 | Mapped | Phase 2 |
| v0.5 Cross-cutting concerns | FR-075, FR-076, FR-107, FR-102 | Mapped | |
| v0.5 How to connect to a product | FR-057, 04 Interfaces | Mapped | |
| v0.5 Decision: no built-in chat | 02 Out of scope; FR-115 | Mapped | Ask-a-question box parked |
| v0.5 Reaching users on Microsoft 365 Copilot | FR-089, FR-092, DEP-004 | Mapped | |
| v0.5 Worked example: critical buildings | FR-109, FR-083, FR-093 | Mapped | Uses BUUK data, so only its pattern is used until the data rules are agreed |
| v0.5 Worked example: Historic Flood Map | JRN-001, JRN-008, 13 To-be | Mapped | |
| v0.5 Where people are involved | 13 To-be steps 2, 5, 6 | Deferred | Narrative only; covered by the to-be steps |
| v0.5 Other sources (UKPN outage feed, SSSI) | FR-015 | Mapped | Polling parked in Phase 4 |
| v0.5 Prior art; Build-versus-adopt checkpoint | CAP-025, ASM-011, DEP-006 | Mapped | The spike itself is outside this idea |
| v0.5 Permissions: who can see a product; Rules; How each door enforces it | FR-051, FR-069 to FR-073, RULE-008 | Mapped | |
| v0.5 Data rules for the prototype and pilot | FR-005, SEC-002, SEC-009, FR-011, NFR-009 | Mapped | |
| v0.5 Open points and recommendations | DEC-004, FR-048, FR-031, FR-081, NFR-010 | Mapped | |
| v0.5 Still to decide | ASM-006, ASM-008, FR-090, NFR-003, ASM-005 | Mapped | |
| v0.5 To confirm with others | DEP-003, DEP-004 | Mapped | |
| v0.5 Gaps to design before this is a full platform | FR-080, FR-104, FR-101, FR-103, FR-106, FR-093 | Mapped | Contracts in CI (Bitol) deferred to Phase 4 with FR-114 |
| v0.5 Later features from the reference portals | FR-110 to FR-114 | Mapped | |
| v0.5 Redrafted sections to review | 09 Q and ASM items | Deferred | The owner reviews the redrafted parts at Gate 2; nothing to build |
| scope Phases and gates | 11 Epics and Release acceptance | Mapped | |
| scope Phase 1 scope (In, Later, Out for now) | 02 In scope and Out of scope | Mapped | |
| scope Epics at a glance; Epics 1 to 11 | EPIC-001 to EPIC-020, DEC-010 | Mapped | Split by phase |
| scope Build order and open decisions | 11 Epics, Q-001, Q-002, Q-003 | Mapped | |
| brief How to use this brief; Ground rules | 11 Definition of Done, NFR-011, NFR-012 | Mapped | |
| brief Data rules for the prototype | RULE-001, SEC-009, FR-005, FR-011 | Mapped | |
| brief Slice 0 | EPIC-001, FR-001 | Mapped | |
| brief Slice 1 | EPIC-002, FR-002 to FR-008 | Mapped | |
| brief Slice 2 | FR-009, FR-010, INT-001 | Mapped | |
| brief Slice 3 | FR-018 | Mapped | |
| brief Slice 4 | FR-021 to FR-024 | Mapped | |
| brief Slice 5 | FR-019, FR-020, FR-025, FR-072 | Mapped | |
| brief Slice 6 | FR-028 to FR-033 | Mapped | |
| brief Slice 7 | FR-038 to FR-042 | Mapped | |
| brief Slice 8 | FR-050 to FR-060, FR-066 to FR-069 | Mapped | |
| brief Slice 9 | FR-108, FR-109 | Mapped | |
| brief Slice 10 | FR-082 to FR-087 | Mapped | |
| brief Slice 11 | FR-094 to FR-096 | Mapped | |
| brief Slice 12 and the demo script | EPIC-009, US-044 | Mapped | |
| brief Acceptance for Phase 0 | 11 Release acceptance | Mapped | |
| brief Questions for the first run | NFR-003, NFR-004, DEC-002 | Mapped | File size answered at Analyse (20 MB zip); Airflow and Cosmos versions answered by the install check |
