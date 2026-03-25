# CML API Documentation - Quick Reference Map

> **For AI assistants:** Read this file first to navigate the documentation efficiently.
> Each section tells you which file to read for a specific topic.
> You do NOT need to read all files - pick only what's relevant to your task.

## Start Here

| Need to understand... | Read this file |
|---|---|
| Overall CML architecture | [PROJECT.md](PROJECT.md) |
| How data pipelines work | [PROJECT.md](PROJECT.md) → "Key Concepts" section |
| What client types exist | This file → sections below |

## API Endpoints (by domain)

| Domain | File | What it covers |
|---|---|---|
| User authentication & management | [endpoints/users.md](endpoints/users.md) | Login, create users, enable/disable |
| Project management | [endpoints/projects.md](endpoints/projects.md) | CRUD projects, assign users |
| Orchestrations (pipelines) | [endpoints/orchestrations.md](endpoints/orchestrations.md) | CRUD pipelines, run, enable/disable, data endpoints |
| Tokens & auth configs | [endpoints/components.md](endpoints/components.md) | API tokens, embed tokens, auth configurations |
| Organization info | [endpoints/organizations.md](endpoints/organizations.md) | Org details, run logs |
| Job execution | [endpoints/jobs.md](endpoints/jobs.md) | Job status, health reports |
| Testing & playground | [endpoints/playground.md](endpoints/playground.md) | Test actions, mock data, output sink |
| Transformation engines | [endpoints/transformation-engines.md](endpoints/transformation-engines.md) | dbt and other external engines |
| Data source init | [endpoints/datalook.md](endpoints/datalook.md) | Datalook initialization |
| E-commerce integrations | [endpoints/agnostic-fetch.md](endpoints/agnostic-fetch.md) | Shoptet integration |

## Schemas - Input Clients (data sources)

> Input clients define HOW data is read from external sources.

| Client Type | File | Use case |
|---|---|---|
| Shared types & pagination | [schemas/input-clients/_types.md](schemas/input-clients/_types.md) | Pagination, params, response handlers |
| HTTP (REST API) | [schemas/input-clients/http.md](schemas/input-clients/http.md) | Most common - API calls |
| FTP/SFTP | [schemas/input-clients/ftp.md](schemas/input-clients/ftp.md) | File downloads |
| Google BigQuery | [schemas/input-clients/google-bigquery.md](schemas/input-clients/google-bigquery.md) | BigQuery SQL queries |
| Snowflake | [schemas/input-clients/snowflake.md](schemas/input-clients/snowflake.md) | Snowflake SQL queries |
| MSSQL | [schemas/input-clients/mssql.md](schemas/input-clients/mssql.md) | SQL Server queries |
| ClickHouse | [schemas/input-clients/clickhouse.md](schemas/input-clients/clickhouse.md) | ClickHouse queries |

## Schemas - Output Clients (destinations)

> Output clients define HOW data is written to external destinations.

| Client Type | File | Use case |
|---|---|---|
| Shared types & handlers | [schemas/output-clients/_types.md](schemas/output-clients/_types.md) | Output formats, result hooks, enums |
| HTTP (REST API) | [schemas/output-clients/http.md](schemas/output-clients/http.md) | Send to APIs (per-item or batch) |
| FTP/SFTP | [schemas/output-clients/ftp.md](schemas/output-clients/ftp.md) | File uploads |
| Google Cloud Storage | [schemas/output-clients/google-cloud-storage.md](schemas/output-clients/google-cloud-storage.md) | GCS file upload |
| Google BigQuery | [schemas/output-clients/google-bigquery.md](schemas/output-clients/google-bigquery.md) | Load into BigQuery |
| Snowflake | [schemas/output-clients/snowflake.md](schemas/output-clients/snowflake.md) | Load into Snowflake |
| PostgreSQL | [schemas/output-clients/postgres.md](schemas/output-clients/postgres.md) | Load into Postgres |
| ClickHouse | [schemas/output-clients/clickhouse.md](schemas/output-clients/clickhouse.md) | Load into ClickHouse |
| Facebook Custom Audience | [schemas/output-clients/facebook.md](schemas/output-clients/facebook.md) | Ad audience management |

## Schemas - Transformation Clients

> Transformation clients reshape data between input and output.

| Client Type | File | Use case |
|---|---|---|
| Mapper | [schemas/transformation-clients/mapper.md](schemas/transformation-clients/mapper.md) | Field mapping, value translation |
| Flatter | [schemas/transformation-clients/flatter.md](schemas/transformation-clients/flatter.md) | Flatten nested structures |

## Schemas - Actions (pipeline steps)

> Actions are the building blocks of orchestrations.

| Action Type | File | What it does |
|---|---|---|
| Common types | [schemas/actions/common.md](schemas/actions/common.md) | Shared action config, status, results |
| Input action | [schemas/actions/input-action.md](schemas/actions/input-action.md) | Read data from source |
| Output action | [schemas/actions/output-action.md](schemas/actions/output-action.md) | Write data to destination |
| Transformation action | [schemas/actions/transformation-action.md](schemas/actions/transformation-action.md) | Transform data |
| Trigger action | [schemas/actions/trigger-action.md](schemas/actions/trigger-action.md) | Run external engines (dbt, etc.) |

## Schemas - Other

| Domain | File | What it covers |
|---|---|---|
| Common enums | [schemas/common/enums.md](schemas/common/enums.md) | All shared enum types |
| Shared utility types | [schemas/common/shared-types.md](schemas/common/shared-types.md) | Pagination, data containers |
| Auth credentials | [schemas/auth/credentials.md](schemas/auth/credentials.md) | Login request/response |
| Auth types | [schemas/auth/auth-types.md](schemas/auth/auth-types.md) | API key, OAuth, Azure, Google, etc. |
| Users | [schemas/users.md](schemas/users.md) | User data types |
| Projects | [schemas/projects.md](schemas/projects.md) | Project data types |
| Organizations | [schemas/organizations.md](schemas/organizations.md) | Organization data types |
| Orchestrations | [schemas/orchestrations.md](schemas/orchestrations.md) | Pipeline config & run types |
| Jobs | [schemas/jobs.md](schemas/jobs.md) | Job execution types |
| Tokens | [schemas/tokens.md](schemas/tokens.md) | API & embed token types |
| Connections | [schemas/connections.md](schemas/connections.md) | DB/service connection configs |
| Datalook | [schemas/datalook.md](schemas/datalook.md) | Data source init templates |
| Transformation engines | [schemas/transformation-engines.md](schemas/transformation-engines.md) | dbt integration types |
| Playground mock | [schemas/playground-mock.md](schemas/playground-mock.md) | Test mock data |
| Agnostic Fetch | [schemas/agnostic-fetch.md](schemas/agnostic-fetch.md) | E-commerce integration |

## Common AI Tasks - Where to Look

| Task | Files to read |
|---|---|
| "Create an orchestration" | `endpoints/orchestrations.md` + `schemas/orchestrations.md` + `schemas/actions/` |
| "Configure HTTP input" | `schemas/input-clients/http.md` + `schemas/input-clients/_types.md` |
| "Set up Snowflake output" | `schemas/output-clients/snowflake.md` + `schemas/connections.md` |
| "Map fields between input/output" | `schemas/transformation-clients/mapper.md` |
| "Authenticate to an API" | `schemas/auth/auth-types.md` + `endpoints/components.md` |
| "Run a pipeline" | `endpoints/orchestrations.md` (run endpoint) |
| "Check job status" | `endpoints/jobs.md` + `schemas/jobs.md` |
