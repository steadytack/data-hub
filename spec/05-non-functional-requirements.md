# 05 Non-functional requirements

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-05 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

Measurable where it matters, scaled to tier 2 for Phase 0 and tier 3 from Phase 1 (D-008); the template's standards apply and are not repeated.

| NFR | Category | Requirement | Threshold | Verification | Source |
|---|---|---|---|---|---|
| NFR-001 | usability | The stack starts with one command and the demo script runs start to finish on a clean stack | One command each; the demo in about 15 minutes, no manual fixes | TEST-001, TEST-044 | brief slice 0, slice 12 |
| NFR-002 | resources and cost | Memory for the whole stack during a flood map run | At most 8 GB | TEST-101 | definition, Deployment |
| NFR-003 | performance | Spatial join of flood map areas with site polygons of one network type on the real data | Under 10 seconds (ASM-010); the actual figure recorded either way | TEST-102 | brief slice 9 |
| NFR-004 | performance | Full flood map run, download to gold, on the real 20 MB file | At most 30 minutes; file size, load time and silver time recorded | TEST-103 | brief Questions for the first run |
| NFR-005 | performance | Catalogue list and product page with 50 products | 95 % of loads within 2 seconds on the local stack | TEST-104 | definition, Interfaces |
| NFR-006 | performance | MCP `describe_product` and a 1,000-row `query_products` | Describe within 2 seconds; query within 10 seconds | TEST-105 | brief slice 10 |
| NFR-007 | accessibility | Every page keyboard usable with visible focus, labelled inputs, 4.5:1 contrast; tier and freshness as text, never by colour alone (DEF-08) | 0 serious or critical axe findings | TEST-106 | DEF-08 |
| NFR-008 | backup and recovery | Data and catalogue recoverable after a failure (Phase 1) | Up to 24 hours of data lost at most; restored within 1 hour | TEST-047, TEST-119 | DEF-06 |
| NFR-009 | privacy | Audit and usage events are kept for 12 months, then purged | Nothing older than 12 months and one day | TEST-107 | v0.5 Data rules |
| NFR-010 | maintainability | Airflow, Cosmos and dbt pinned with Airflow's constraints file; upgraded about every six months plus security patches, on a copy first; DAG factory contract tests catch any change in generated DAGs | 0 unexpected DAG changes after an upgrade | TEST-108 | v0.5 Open points (Airflow versioning) |
| NFR-011 | maintainability | No AI-generated content: descriptions, metadata and text come from rules, harvested metadata or people | 0 model clients in any image or lock file | TEST-109 | brief Ground rules |
| NFR-012 | portability | All storage is Postgres; no other store or file format for tables | 0 other stores | TEST-109 | brief Ground rules |
| NFR-013 | observability | Every run, check, promotion and override is in the catalogue or audit log; containers have health checks; heartbeat on | 100 % of runs with a run record and step logs | TEST-023, TEST-006 | brief Ground rules |
| NFR-014 | portability | Runs under Docker Compose on Windows (Docker Desktop) and on a Linux host with no change to the Compose file | Both start clean | TEST-001 on both | scope Decisions (Hosting) |
| NFR-015 | usability | UK English, UK dates, Europe/London for every date and freshness calculation (DEF-07) | All dates shown day, month, year (14 Nov 2026) | TEST-013, TEST-031 | DEF-07 |
