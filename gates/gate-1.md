# Gate 1: Data Hub (Trusted Data Products) (Application Definition)

Status: approved 2026-11-14. Date: 2026-11-14. Definition version 1.0.

## Done
- Three BUUK documents imported unchanged to `source/v0.5/`, `source/v0.5-scope/`, `source/v0.5-brief-001/` (47 headings and 14 tables; 24 and 2; 24 and 0, matching the files).
- The real flood map inspected (`source/samples/historic-flood-map-schema.md`): 20 MB zip, 21,578 MultiPolygons in EPSG:27700, only three attribute columns (`objectid`, `shape_leng`, `layer`); version file says "This is the September 2026 release."
- `application-definition.md` (source summary, problem, users, scope, tier, deployment, interfaces, template, data and security, operations, all eight lenses, 11 assumptions, 7 risks) and `overview.md` for sponsors.
- `check_spec.py data-hub --definition`: **OK, 0 blockers, 0 warnings**.

## Recommended
- **Tier 3** (shared pilot). **Deployment:** local Docker Compose, ports on 127.0.0.1. **Interfaces:** web + non-visual (SQL, export, MCP, Airflow).
- **Template:** `web-app` (CAT-006); runner-up web-app + etl-pipeline (its runner and SQLite are replaced by Airflow and dbt). New components to import and approve: Airflow, Cosmos, dbt-postgres, MCP Python SDK.

**Coverage:** scope Clear, users Clear, data Partial, integrations Partial, non-functional Partial, risks Clear.

## Template features (web-app; change only what you want different)
- Share: **off** (no public links to data products)
- Theme changer: on
- Help with Guides: on
- Feedback: on; Problem, Suggestion, Question: **store** in the app's admin list (no mail service set up)
- Heartbeat: on

## Decisions for you
1. **Spec depth.**
   - A. Phase 0 (slices 0 to 12) in full; Phases 1 to 3 outlined at epic and story level; Phase 4 parked. (Recommended: the source gates each phase on the previous one.)
   - B. Phase 0 and Phase 1 in full.
   - C. All phases in full.
2. **Maturity tier.**
   - A. Tier 3, shared pilot. (Recommended: several roles and access enforced on four doors.)
   - B. Tier 2, personal tool for the prototype, raised to 3 for the pilot.
3. **New components.** Airflow, Cosmos, dbt-postgres and the official MCP Python SDK are not in the catalogue (POL-10).
   - A. Import and approve all four in Spec, versions fixed after an install check. (Recommended: the source requires them.)
   - B. Use `fastmcp` instead of the official SDK for MCP.

## Answered from your defaults
- Deployment local Docker Compose (DEF-02; also the source's hosting decision); ports 127.0.0.1 (POL-06).
- Brand unbranded, template design file, quiet UI (DEF-09); accessibility (DEF-08); UK locale (DEF-07).
- Backups: nightly dump (DEF-06). Repository private (DEF-14). Licence MIT (DEF-13), to be confirmed at Publish since this is BUUK work.
- Template features from DEF-26, with Share off and feedback stored (D-006).

## Assumed, not asked
ASM-002 React front end from the template; ASM-003 Airflow 3.x with matching Cosmos and dbt-postgres, pinned; ASM-004 MCP identity via a personal access token until Entra; ASM-005 stewards and admins do not see restricted data unless granted, products never hidden; ASM-006 first trust-score weights and thresholds in YAML; ASM-007 Fresh within 100 days of release, Late to 130, then Stale; ASM-008 minimum tier passed by the MCP caller, floor Exploratory; ASM-009 about 200 seeded synthetic site polygons overlapping flood areas; ASM-010 acceptable join time under 10 s; ASM-011 build-versus-adopt spike out of scope.

## Risks
Embedded Airflow weight and version compatibility (RISK-001, 002); access enforced unevenly across doors (RISK-003); MCP data reaching the model provider (RISK-004); thin flood map attributes leave the dictionary owner-supplied (RISK-005); BUUK material in the factory repository (RISK-006).

## Next
Spec, in a fresh session (`/factory resume data-hub`): the 13-document pack, composition and delivery plan by slice; expect at most one more question round.

## Reply
2026-11-14: "1. C, 2.B, 3.A,". Decision 1 changed to C (all phases in full, D-007), 2 changed to B (Tier 2 then 3, D-008), 3 as recommended (D-009); everything else accepted as recommended (D-010).
