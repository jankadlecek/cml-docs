# Organizations API

> **Context for AI:** Organizations are the top-level entity in CML. They group users and projects.
> The run-logs endpoint provides execution history across all projects in the organization.
>
> **Related schemas:** [organizations](../schemas/organizations.md)

## Endpoints

### `GET /api/rest/v1/organizations/my-organization`

**MyOrganization**

**Responses:**

- `200`: Ok → `IOrganizationApi`

---

### `GET /api/rest/v1/organizations/my-organization/run-logs`

**ListRunLogs**

API to list all Orchestration Run Logs assigned to given organization.

**Parameters:**

- `createdAtFrom` (query, string) *(required)*: Expects timestamp - filters jobs using createdAt (included)
- `createdAtTo` (query, string): Expects timestamp - filters jobs created to (excluded)
- `projectId` (query, string): Expects UUID of the project
- `orchestrationId` (query, string): Expects UUID of the orchestration
- `limit` (query, string): Page limit
- `offset` (query, number): Page offset
- `state` (query, string): State

**Responses:**

- `200`: Ok → `IListOrchestrationRunResponse`

---

