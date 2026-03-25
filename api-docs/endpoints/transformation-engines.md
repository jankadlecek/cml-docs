# Transformation Engines API

> **Context for AI:** Transformation Engines are external processing engines (like dbt)
> integrated into CML. They run transformation logic outside the core CML pipeline.
> Each engine has profiles defining connection and execution parameters.
>
> **Related schemas:** [transformation-engines](../schemas/transformation-engines.md)

## Endpoints

### `GET /api/rest/v1/projects/{projectId}/transformation-engines`

**ListTransformationEngines**

Provides list of Transformation Engine for given .

**Parameters:**

- `projectId` (path, string) *(required)*: - required param used to associate new model to specific project
- `limit` (query, string): 
- `offset` (query, number): 

**Responses:**

- `200`: Ok → `ITransformationEngineListResponse`

---

### `POST /api/rest/v1/projects/{projectId}/transformation-engines`

**InitTransformationEngine**

Provides Transformation Engine initialization.

**Parameters:**

- `projectId` (path, string) *(required)*: - required param used to associate new model to specific project

**Request Body** (`application/json`): `IInitTransformationEngineBodyApi`

**Responses:**

- `200`: Ok → `ITransformationEngineApi`

---

### `GET /api/rest/v1/projects/{projectId}/transformation-engines/{transformationEngineId}/profiles`

**ListTransformationEngineProfiles**

Provides List Transformation Engine Profiles.

**Parameters:**

- `projectId` (path, string) *(required)*: - required param used to associate new model to specific project
- `transformationEngineId` (path, string) *(required)*: 
- `limit` (query, string): 
- `offset` (query, number): 

**Responses:**

- `200`: Ok → `ITransformationEngineProfilesListResponse`

---

### `POST /api/rest/v1/projects/{projectId}/transformation-engines/{transformationEngineId}/profiles`

**AddTransformationEngineProfile**

Provides add Profile for Transformation Engine.

**Parameters:**

- `projectId` (path, string) *(required)*: - required param used to associate new model to specific project
- `transformationEngineId` (path, string) *(required)*: - required param used to associate new model to specific Transformation Engine model

**Request Body** (`application/json`): `IProfileCreateTransformationEngineApi`

**Responses:**

- `200`: Ok → `IProfileTransformationEngineApi`

---

