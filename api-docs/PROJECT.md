# CML (Cloud Middleware Layer) - Architecture Guide

> **For AI assistants:** This document explains the overall architecture of CML.
> Read this first to understand the system before diving into specific endpoints or schemas.

## What is CML?

CML (Cloud Middleware Layer) is a **data integration and orchestration platform**. It enables users to build data pipelines that:

1. **Extract** data from various sources (APIs, databases, files)
2. **Transform** the data (mapping fields, flattening structures)
3. **Load** the data into destinations (databases, APIs, files, cloud storage)

Think of it as an ETL/ELT tool with a REST API for configuration and execution.

## Core Architecture

```
Organization
└── Project
    ├── Orchestration (= data pipeline)
    │   ├── Input Action      → reads data from a source
    │   ├── Transformation Action → transforms data (map/flatten)
    │   ├── Output Action      → writes data to destination
    │   └── Trigger Action     → runs external tools (dbt, etc.)
    │
    ├── Transformation Engine  → external processing (dbt profiles)
    └── Users (with project-scoped permissions)
```

## Key Concepts

### Organization
The top-level entity. Groups users and projects. Each user belongs to one organization.

### Project
The main organizational unit. Contains orchestrations and transformation engines. Users are assigned to projects with specific permission scopes.

### Orchestration
A **data pipeline definition**. Each orchestration contains a sequence of **actions** that define the data flow. Orchestrations can be:
- Enabled/disabled
- Run manually or via API
- Expose data endpoints (for receiving webhook data)

When an orchestration runs, it creates a **Run** record tracking execution status.

### Actions (the pipeline steps)

Actions are the building blocks of an orchestration. They execute in sequence:

#### Input Action
Reads data from an external source. Uses an **Input Client**:
- **HTTP** - REST API calls (most common), with pagination, auth, retries
- **FTP** - File download from FTP/SFTP
- **Google BigQuery** - SQL queries against BigQuery
- **Snowflake** - SQL queries against Snowflake
- **MSSQL** - SQL queries against SQL Server
- **ClickHouse** - SQL queries against ClickHouse

#### Transformation Action
Transforms data between input and output. Uses a **Transformation Client**:
- **Mapper** - Field-by-field mapping with value translations (rename, transform, lookup)
- **Flatter** - Flattens nested/hierarchical data into flat records

#### Output Action
Writes data to a destination. Uses an **Output Client**:
- **HTTP** - Send data to REST APIs
- **SingleHTTP** - Send all data in one request
- **FTP** - Upload files to FTP/SFTP
- **Google Cloud Storage** - Upload to GCS
- **Google BigQuery** - Load into BigQuery tables
- **Snowflake** - Load into Snowflake tables
- **PostgreSQL** - Load into Postgres tables
- **ClickHouse** - Load into ClickHouse tables
- **Facebook Custom Audience** - Upload audience data
- **Response** - Return data as API response (for data endpoints)

#### Trigger Action
Invokes external processing engines:
- **Transformation Engine** - Run CML transformation engine
- **dbt** - Run dbt models
- **DataForm** - Run Google DataForm
- **Keboola** - Trigger Keboola jobs

### Auth (Authentication Configurations)
Stored credentials for connecting to external services. Types include:
- API Key, Basic-to-Bearer, Azure Service Principal, Google Credentials, Custom

### Connections
Database/service connection parameters (host, port, credentials) for:
- FTP, Snowflake, PostgreSQL, MSSQL, ClickHouse

### Jobs
Execution units created when orchestrations run. Workers pick up jobs, execute actions, and report health status.

### Tokens
API access tokens. Types:
- Standard API tokens for authentication
- Embed tokens for embedded UI components

### Datalook
Data exploration/initialization module for setting up source system connections and templates.

### Agnostic Fetch
E-commerce platform integration module (currently Shoptet).

## Data Flow Example

```
1. User creates a Project
2. User configures Auth (e.g., API key for source API)
3. User creates an Orchestration with:
   a. Input Action (HTTP client → fetch orders from API)
   b. Transformation Action (Mapper → rename/transform fields)
   c. Output Action (Snowflake client → load into warehouse)
4. User enables and runs the Orchestration
5. CML creates a Job, executes actions in sequence
6. Run status and results are tracked
```

## API Structure

The API is organized by domain:
- `/api/rest/v1/users/*` - User management & auth
- `/api/rest/v1/projects/*` - Project CRUD & user assignment
- `/api/rest/v1/projects/{id}/orchestrations/*` - Pipeline management
- `/api/rest/v1/projects/{id}/transformation-engines/*` - External engines
- `/api/rest/v1/components/*` - Tokens & auth configs
- `/api/rest/v1/organizations/*` - Organization info & logs
- `/api/rest/v1/jobs/*` - Job execution & health
- `/api/rest/v1/playground/*` - Action testing & mock data
- `/api/rest/v1/datalook/*` - Data source initialization
- `/api/rest/v1/agnostic-fetch/*` - E-commerce integrations

## File Structure of This Documentation

```
api-docs/
├── PROJECT.md              ← You are here (architecture overview)
├── _overview.md             ← Quick reference map of all files
├── endpoints/               ← API endpoint documentation
│   ├── users.md
│   ├── projects.md
│   ├── orchestrations.md
│   ├── components.md
│   ├── organizations.md
│   ├── jobs.md
│   ├── playground.md
│   ├── transformation-engines.md
│   ├── datalook.md
│   └── agnostic-fetch.md
└── schemas/                 ← Data type definitions
    ├── common/              ← Shared enums and utility types
    ├── auth/                ← Authentication schemas
    ├── input-clients/       ← Input client configs (by type)
    ├── output-clients/      ← Output client configs (by type)
    ├── transformation-clients/ ← Mapper & Flatter configs
    ├── actions/             ← Action configuration schemas
    ├── orchestrations.md
    ├── jobs.md
    ├── tokens.md
    ├── connections.md
    ├── users.md
    ├── projects.md
    └── ...
```
