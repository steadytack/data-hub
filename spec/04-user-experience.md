# 04 User experience

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-04 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

## Interfaces and navigation
- **Web (CMP-002):** a top bar with Catalogue, My work (owner's missing fields and review queue; steward's findings), Runs, Admin (admin only), and the user menu (tokens, theme, help, feedback, sign out). The catalogue opens each product's page; the page's tabs carry everything about one product. No map until Phase 2.
- **Non-visual:** SQL (read-only logins), file export (from the Access tab or the API), MCP (stdio and streamable HTTP), and from Phase 2 OGC API Features, REST and OData. Each door's connection details are on the Access tab.
- **Airflow's UI is never shown** (SEC-003); run history is the app's own page.

## Screens
| SCR | Screen | Purpose | FRs | Main elements and actions | States |
|---|---|---|---|---|---|
| SCR-001 | Sign in | Named sign-in | FR-066 | Email, password, sign in; Phase 3: "Sign in with BUUK" (FR-080) | Empty, signing in, wrong details (one generic message), deactivated |
| SCR-002 | Catalogue | Find products | FR-050, FR-051 | Search box; managed table of products; header filters for domain, owner, tier, access level, spatial | Empty catalogue, loading, results, no matches |
| SCR-003 | Product page | Judge and use one product | FR-052 to FR-057, FR-073, FR-065 | Header: tier, access level, freshness, owner (all as text labels); tabs Overview, Data dictionary, Quality, Known issues, Access; Phase 2 map preview and usefulness prompt | Loading, Not set fields, no access (data parts hidden with "Ask the owner for access"), failed last run banner |
| SCR-004 | Report an issue | Raise an issue from the Known issues tab | FR-058 | Dialog: scope, description, suggested severity, send | Empty, validation error, sent |
| SCR-005 | Findings (steward) | Triage open findings and reported issues | FR-059, FR-030, FR-033 | Managed table; row actions triage, set severity, assign owner, fix link, resolve; override tier with reason; sign off Certified | Empty, loading, saved, refused |
| SCR-006 | Missing fields and review queue (owner) | Fill gaps and review flagged items | FR-060, FR-034, FR-035, FR-036 | Managed table of missing and flagged items with evidence; edit in place; accept, edit, reject; bulk approve (Phase 1); reminders | Empty ("Nothing to review"), loading, saved |
| SCR-007 | Runs | See every run and step | FR-042, FR-049 | Managed table of runs; run detail with steps, durations and logs; failure links to its step; Phase 2 DAG graph with rerun and retry | Empty, running, succeeded, failed, skipped |
| SCR-008 | Admin: users | Manage users and roles | FR-067, FR-077 | Managed table; add, deactivate, change role | Empty, saved, refused |
| SCR-009 | Admin: groups | Manage groups and members | FR-068 | Managed table of groups; members sub-table; add and remove | Empty, saved |
| SCR-010 | Product access settings | Set access level, groups, named grants | FR-069, FR-070 | Level selector, group picker, grants sub-table with expiry; licence ceiling note | Saved, refused (ceiling), refused (not owner) |
| SCR-011 | Licence register | Keep licence entries | FR-004 | Managed table; add and edit an entry | Empty, saved, validation error |
| SCR-012 | My tokens | Personal access tokens for MCP | FR-078 | Managed table; create (shown once), revoke | Empty, created, revoked |
| SCR-013 | Audit log | Read who did what | FR-008, FR-077 | Managed table with filters on actor, action, product and date | Empty, loading |
| SCR-014 | Feedback (template) | Problems, suggestions and questions stored in the app (D-006) | FR-058 | Template's feedback list | Empty, loading |
| SCR-015 | Pipeline builder (Phase 2) | Build a pipeline in forms | FR-045, FR-046, FR-047, FR-048 | Step library, step form with preview and row count, pipeline view with generated SQL, save to draft, reconcile diff | Draft, previewing, conflict, saved, locked by another owner |
| SCR-016 | Data requests (Phase 2) | Ask for data and triage requests | FR-062 | Request form with matching products; steward backlog table | Empty, match found, logged |
| SCR-017 | Usage dashboard (Phase 3) | Usage per product and door | FR-105 | Charts with a data table view | Empty, loading |
| SCR-018 | Access requests (Phase 3) | Request and decide access | FR-103 | Request button on the Access tab; owner's requests table with approve or decline | Requested, approved, declined |

States cover empty, loading, success, validation and error.

## Record tables
Every list is the template's `ManagedDataTable`; lists that can outgrow a page run on the server (`ServerTable`).

| Screen | Table | Record | Columns (every field) | Shown by default | Header filters | Saved view | Rows |
|---|---|---|---|---|---|---|---|
| SCR-002 | Products | DATA-001 | title, name, description, domain, owner, tier, access level, freshness, spatial, AI use, version, last run, licence | title, domain, owner, tier, access level, freshness | domain, owner, tier, access level, spatial, freshness | per table | server |
| SCR-003 | Data dictionary | DATA-021 | column, type, unit, definition, source of text, null rate, distinct count | column, type, unit, definition | type, definition set | one `layoutKey` for all products | client |
| SCR-003 | Check results | DATA-006 | rule, boundary, severity, passed, value, baseline, run, date | rule, boundary, passed, value | boundary, severity, passed | one `layoutKey` | client |
| SCR-003 | Known issues | DATA-005 | description, scope, type, severity, measure, origin, status, owner, fix link, opened, resolved | description, severity, status, opened | severity, status, origin | one `layoutKey`, status filter carried over | client |
| SCR-005 | Findings | DATA-005 | product, description, scope, type, severity, measure, origin, status, owner, fix link, opened | product, description, severity, status, opened | product, severity, status, origin | per table | server |
| SCR-006 | Missing and flagged items | DATA-021, DATA-005 | product, field, current value, evidence, reason, due date | product, field, evidence, due date | product, reason | per table | server |
| SCR-007 | Runs | DATA-007 | product, trigger, status, started, duration, skip reason, version produced | product, status, started, duration | product, status, trigger | per table | server |
| SCR-007 | Steps | DATA-007 | step, status, started, duration, log link | step, status, duration | status | one `layoutKey` | client |
| SCR-008 | Users | DATA-011 | name, email, role, active, last sign-in, created | name, email, role, active | role, active | per table | client |
| SCR-009 | Groups and members | DATA-012, DATA-013 | group, description, member count; member name, email, added | group, member count | group | per table | client |
| SCR-010 | Named grants | DATA-014 | user, email, granted by, granted on, expires on | user, expires on | expires on | one `layoutKey` | client |
| SCR-011 | Licences | DATA-004 | name, link, attribution, permitted uses, AI permitted, ceiling, manager, expiry | name, AI permitted, ceiling, expiry | AI permitted, ceiling | per table | client |
| SCR-012 | Tokens | DATA-020 | name, created, expires, last used | name, expires, last used | none | per table | client |
| SCR-013 | Audit events | DATA-015 | time, actor, action, item, detail | time, actor, action, item | actor, action, date | per table | server |

Not tables: the product header and Overview tab (one record's facts, a key/value card); the Quality tab's score breakdown (a chart's data view); the Access tab's connection details (copyable fields).

## Journeys as flows
- **JRN-001 (owner):** commit manifest → product appears (Named) → Runs shows the run → product page shows Not set fields → My work: fill them → Promote (refused with reasons, or published).
- **JRN-002 (consumer):** Catalogue → filter → product page → header and tabs → Access tab → copy SQL details or MCP snippet, or download.
- **JRN-006 (steward):** My work → Findings → triage → override with reason or sign off Certified.
- **JRN-007 (admin, owner):** Admin → users and groups; owner: product → Access settings → level, groups, grants.
- **JRN-009 (consumer):** product page → Known issues → Report → appears in the list.
- JRN-003, JRN-004 and JRN-005 run outside the UI except for copying details from the Access tab.

## Responsive and mobile behaviour
Desktop first (all users work at desktops). Pages stay usable down to 768 px; tables scroll sideways inside their frame; no phone-specific layout.

## Accessibility
DEF-08 (NFR-007): keyboard use, visible focus, labelled inputs, 4.5:1 contrast. Tier, freshness and access level are words, never colour alone; icon-only controls have tooltips; axe in a real browser on every page.

## Design system
Unbranded, the web-app template's design file, quiet UI (about-me): small buttons, light weights, neutral colours, no explanatory text on screens. BUUK branding may be added before the pilot (definition).
