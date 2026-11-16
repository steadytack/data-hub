# Install check: Airflow, Cosmos, dbt-postgres and the MCP SDK (2026-11-14)

Run in this session in throwaway `python:3.12-slim` containers (Docker Desktop), as D-009 asks. Nothing ran against any project's stack. Versions are the latest on PyPI on the day.

| Check | Result |
|---|---|
| A. `apache-airflow[postgres]==3.4.0`, `astronomer-cosmos==1.16.0`, `dbt-core==1.12.7`, `dbt-postgres==1.11.0` in one environment with Airflow's `constraints-3.4.0/constraints-3.12.txt` (752 lines) | **Failed**: ResolutionImpossible. "dbt-core 1.12.7 depends on pathspec<1.1 and >=0.9", which Airflow's constraints do not allow |
| B1. `apache-airflow[postgres]==3.4.0` and `astronomer-cosmos==1.16.0` with the constraints file | **Passed**: installed; `pip check` "No broken requirements found"; `import airflow, cosmos` and `from cosmos import DbtTaskGroup, ProjectConfig, ExecutionConfig` worked (airflow 3.4.0, cosmos 1.16.0) |
| B2. `dbt-core==1.12.7` and `dbt-postgres==1.11.0` in their own virtual environment | **Passed**: installed; `pip check` clean; `dbt --version` reported core 1.12.7. Resolved: dbt-adapters 1.24.5, dbt-common 1.39.0, psycopg2-binary 2.9.13 |
| C. The web-app template's backend requirements (`templates/web-app/backend/requirements-dev.txt`) plus `mcp==2.3.0` | **Passed**: installed; `pip check` clean; mcp 2.3.0 with fastapi 0.163.0, pydantic 2.14.3, starlette 1.10.0; `mcp.server` offers `MCPServer`, `stdio` and `streamable_http` |

Constraint pins seen: `apache-airflow-providers-postgres==7.1.1`, `psycopg==3.3.6`, `psycopg2-binary==2.9.13`.

**What it means (DEC-004):** the Airflow image installs Airflow and Cosmos with the constraints file, and dbt in a separate virtual environment inside the same image; Cosmos calls that dbt through its `dbt_executable_path` (execution mode local). The MCP server runs in the backend's image, sharing its service layer.

**Noticed:** `dbt --version` announces that "dbt v2 is available, and is the default for new installations". The pack stays on dbt Core 1.x, the line dbt-postgres 1.11 and Cosmos 1.16 were installed with (RISK-008).

**Not run:** the libraries' own test suites (`catalogue.py validate` was not run in this session); a real DAG run; dependency vulnerability scans.
