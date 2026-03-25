# Jobs API

> **Context for AI:** Jobs represent individual execution units within orchestration runs.
> They report health status and can be marked as finished. The health-report endpoint
> is used by workers to report their status back to the master API.
>
> **Related schemas:** [jobs](../schemas/jobs.md)

## Endpoints

### `POST /api/rest/v1/jobs/{jobId}/finish`

**Finish**

**Parameters:**

- `jobId` (path, string) *(required)*: 

**Request Body** (`application/json`): `IJobFinishApi`

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/jobs/health-report`

**HealthReport**

**Request Body** (`application/json`): `IJobHealthReport`

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/jobs`

**ActiveJobs**

API to show all active jobs.
For each job, it will show the current running actions and the status of completed actions.

**Responses:**

- `200`: Ok

---

