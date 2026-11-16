# 07 Security and permissions

| Doc | Version | Status | Updated | Baseline |
|---|---|---|---|---|
| SPEC-07 | 0.1 | draft | 2026-11-14 | application-definition 1.0 |

The baseline is the factory's `standards/security.md`: the tier 2 row for Phase 0, and the tier 3 row (per-user accounts and roles, a role and action matrix enforced server-side and tested, size and rate limits, who did what and when, a dependency scan in CI, licence review of imported assets) from Phase 1 (D-008). Exposure stays on 127.0.0.1 until Phase 3, which adds a reverse proxy with TLS and documented exposure.

## Authentication
- **Phases 0 to 2:** named accounts with email and a bcrypt-hashed password; short-lived JWT in an HTTP-only, SameSite cookie (the template's sign-in, extended to four roles). No shared logins.
- **MCP:** a personal access token per user (FR-078), sent as a bearer token over HTTP or set in the stdio client's environment; the server acts for that user.
- **SQL:** a read-only Postgres login per user, created and kept in step by the access service (FR-072); its password is shown once on the Access tab and can be reset there.
- **Phase 2:** service identities with tokens for agents (FR-079). **Phase 3:** Entra single sign-on replaces passwords for people (FR-080).

## Roles
| ROLE | Role | Who | Can |
|---|---|---|---|
| ROLE-001 | Consumer | Analysts, GIS users, AI assistant users | Search and read product pages; read data where the access rule allows; download; report issues; create own tokens |
| ROLE-002 | Owner | Pipeline owners | Everything a consumer can; register and edit manifests for their products; fill missing fields; set access levels and grants; promote their own products; roll back |
| ROLE-003 | Steward | Data stewards (domain stewards and their deputies from Phase 1, FR-116) | Everything a consumer can; triage issues and findings; override tiers with a reason; sign off Certified, for products in their own domains; manage the licence register |
| ROLE-004 | Admin | Platform administrators | Everything a consumer can; manage users, roles, groups and members; read the audit log; run backups and restores |
| ROLE-005 | Platform | The scheduled pipelines (Airflow tasks) | Write bronze, silver and gold; write run, check, profile and issue records; nothing else |
| ROLE-006 | Service identity (Phase 2) | Agents and internal apps | Read the products it is granted, within limits |

Roles say what someone can do on the platform; the access level says which data they can read (RULE-008). A role never grants data access by itself.

## Permissions
| PERM | Action | Resource | Roles | FR | Enforced at | Test |
|---|---|---|---|---|---|---|
| PERM-001 | Search and read product metadata (name, description, owner, tier, page tabs except data) | Every product | ROLE-001, ROLE-002, ROLE-003, ROLE-004 | FR-050, FR-051 | API | TEST-030, TEST-113 |
| PERM-002 | Read product data (preview, SQL, MCP, export, later doors) | A product, per RULE-008 | Any role allowed by the access rule; the product's owner always; ROLE-006 when granted | FR-071, FR-072, FR-073, FR-096 | Access service on every door; Postgres grants | TEST-029 |
| PERM-003 | Register and edit a manifest | Own products | ROLE-002 | FR-002 | API, pipelines repository review | TEST-113 |
| PERM-004 | Promote silver to gold | Own product | ROLE-002 (the product's owner) | FR-032 | API | TEST-020 |
| PERM-005 | Set access level, groups and named grants | Own product | ROLE-002 (the product's owner) | FR-069 | API | TEST-028 |
| PERM-006 | Override a tier, sign off Certified, triage issues | Any product | ROLE-003 | FR-030, FR-033, FR-059 | API | TEST-018, TEST-021 |
| PERM-007 | Report an issue | Any product they can find | ROLE-001, ROLE-002, ROLE-003, ROLE-004 | FR-058 | API | TEST-032 |
| PERM-008 | Manage users, roles, groups and members | Users and groups | ROLE-004 | FR-067, FR-068 | API | TEST-026 |
| PERM-009 | Manage the licence register | Licences | ROLE-003, ROLE-004 | FR-004 | API | TEST-113 |
| PERM-010 | Read the audit log | Audit events | ROLE-004, ROLE-003 (read only) | FR-008 | API | TEST-113 |
| PERM-011 | Create, list and revoke personal access tokens | Own tokens | ROLE-001, ROLE-002, ROLE-003, ROLE-004 | FR-078 | API | TEST-035 |
| PERM-012 | Read run history | Own products (owner); all (steward, admin) | ROLE-002, ROLE-003, ROLE-004 | FR-042 | API | TEST-113 |
| PERM-013 | Start, rerun or roll back a run | Own products (owner); all (admin) | ROLE-002, ROLE-004 | FR-012, FR-049 | API, orchestrator interface | TEST-113 |
| PERM-014 | Fill missing human-supplied fields | Own products | ROLE-002 | FR-060 | API | TEST-034 |
| PERM-015 | Triage, override tiers and sign off Certified; this narrows PERM-006 to the steward's own domains. Assign domain stewards and deputies | Products in the steward's domains; domains (admin) | ROLE-003 (own domains), ROLE-004 | FR-116 | API | TEST-086, TEST-113 |
| PERM-016 | Enable or disable a door, globally or per product | Doors and products | ROLE-004 (globally); ROLE-002 (own products) | FR-127 | API | TEST-090, TEST-113 |
| PERM-017 | Create a connection (name, type, address, secret reference) and test a source | Own connections and products | ROLE-002 (own); ROLE-004 (all) | FR-121, FR-123 | API | TEST-083, TEST-113 |

## Security and privacy requirements
| SEC | Requirement | Threat or reason | Verification | Source |
|---|---|---|---|---|
| SEC-001 | Passwords are hashed with bcrypt; accounts are named; failed sign-ins give one generic message | Credential theft; account guessing | TEST-118 | v0.5 Data rules |
| SEC-002 | Secrets come only from the environment; startup refuses missing or placeholder values; gitleaks runs in CI; no secret in the repository, manifests, logs or this pack (POL-02) | Leaked credentials | TEST-110 | brief Data rules |
| SEC-003 | Every published port binds to 127.0.0.1 (POL-06); the Airflow API server and Airflow's database are not published at all | Exposure of internals; users opening Airflow | TEST-111 | v0.5 Embedded Airflow |
| SEC-004 | Read-only logins hold USAGE on `gold` and SELECT on published views and metadata tables only, with a statement timeout; they cannot read bronze, silver, catalogue or audit | Raw SQL bypassing tier and licence checks | TEST-029 | v0.5 SQL access |
| SEC-005 | The MCP query tool accepts one SELECT statement naming only published views, refuses anything else before running it, and runs it in a read-only transaction under the caller's read-only login with a timeout (DEC-009) | SQL injection through an agent; reaching other schemas | TEST-112, TEST-039 | brief slice 10 |
| SEC-006 | One access service decides data access for every door; there is no second implementation | A door enforcing access differently | TEST-029, TEST-113 | v0.5 Permissions |
| SEC-007 | The audit log is append-only for the app's database role, never holds query results, and is purged after 12 months | Tampering; result data leaking into logs | TEST-006, TEST-107 | v0.5 Data rules |
| SEC-008 | Personal access tokens are stored hashed, shown once, expire within at most 90 days and can be revoked | Token theft | TEST-120 | ASM-004 |
| SEC-009 | Over MCP, only products with AI use on, a licence permitting AI use and public or synthetic data answer (DEC-007) | Restricted data reaching a model provider | TEST-114 | brief Data rules |
| SEC-010 | No source is loaded without a licence register entry; attribution wording travels with every download | Licence breach | TEST-004, TEST-042 | v0.5 Licence register |
| SEC-011 | Session cookies are HTTP-only and SameSite; state-changing requests need the session and a CSRF check (template) | Session theft; cross-site requests | TEST-118 | web-app stack |
| SEC-012 | Per-user rate limits on every door (Phase 1) | Runaway AI-driven queries | TEST-046 | scope Epic 8 |
| SEC-013 | Uploads are limited in size (configurable, 500 MB default) and type (CSV, GeoPackage, checked by content), and land only in bronze (Phase 1) | Malicious or oversized files | TEST-117 | scope Epic 5 |
| SEC-014 | Dependencies are scanned for known vulnerabilities in CI from Phase 1 (tier 3) | Vulnerable libraries | TEST-116 | security standard |
| SEC-015 | Phase 3: HTTPS with a certificate on a domain, Entra authentication on the remote MCP server, and the exposure documented | Interception; unauthenticated remote access | TEST-069 | scope Epic 9 |
| SEC-016 | The pipeline database role (ROLE-005) can write bronze, silver and gold only; owners' dbt models and custom SQL run under it and cannot touch catalogue or audit | An owner's model altering trust records | TEST-115 | v0.5 Pipelines |
| SEC-017 | Source secrets (API keys, tokens, database passwords) live only in the secrets store and are referenced by name; they never appear in a manifest, log, audit event, run history, API response, export or error message | A leaked credential reaching a source system | TEST-082 | CR-001 |
| SEC-018 | A source account is read-only and used only for its named connection; pulls are limited by a timeout and row and time limits per run, and may reach only addresses an admin has registered for connections | A pull altering or overloading a source; a manifest pointing the platform at an arbitrary address | TEST-080, TEST-081 | CR-001 |

## Secrets, exposure and audit
- **Source secrets (Phase 1, SEC-017):** held in Docker secrets mounted into the Airflow image, referenced by connection name; the company's vault replaces them if IT and security require it (Q-004). Never in `.env` examples, manifests or the repository.
- **Secrets:** the database passwords for the app, pipeline and Airflow roles, the Airflow API user's password, Airflow's Fernet key, the JWT secret and the first admin's password hash, all in the git-ignored `.env` (names in doc 10). Read-only SQL login passwords are generated per user and stored only as Postgres hashes.
- **Exposure:** front end, backend, MCP HTTP and the data database's SQL port on 127.0.0.1 (POL-06); nothing else published. Phase 3 changes this only through the reverse proxy (CMP-016), approved at that phase's gate.
- **Audit events:** manifest loads, runs and skips, check results, scores, tier changes, overrides, sign-offs, promotions, rollbacks, issue changes, access-level and grant changes, user, role, group and member changes, sign-ins (Phase 1), token creation and revocation, exports, and MCP calls (row counts only). Retained 12 months.
- **Personal data (POL-09):** user name, email and password hash (FR-066), needed to sign in and to name owners; user IDs in audit and usage events (FR-008, FR-076), needed for accountability. Removed when an admin deletes the account; events purged at 12 months. No personal data in the products themselves in Phases 0 and 1 (public and synthetic only).
- **Supply chain:** the five imported libraries are pinned and approved before use (DEP-002); Airflow's constraints file pins its tree; the images use official bases.

## Residual risks
RISK-003 (uneven access across doors), RISK-004 (MCP data reaching the model provider), RISK-011 (the statement guard), RISK-010 (unapproved libraries), RISK-012 (a leaked source credential) and RISK-013 (pulls loading a production database) in doc 09.
