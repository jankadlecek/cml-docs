# Projects API

> **Context for AI:** Projects are the main organizational unit in CML. Each project contains
> orchestrations (data pipelines), transformation engines, and assigned users.
> Projects belong to an organization and have their own user permission scopes.
>
> **Related schemas:** [projects](../schemas/projects.md), [users](../schemas/users.md)

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

### `POST /api/rest/v1/projects`

**CreateProject**

**Request Body** (`application/json`): `IProjectCreateBody`

**Responses:**

- `200`: Ok → `IProjectApi`

---

### `GET /api/rest/v1/projects`

**ListProjects**

**Parameters:**

- `limit` (query, string): 
- `offset` (query, number): 
- `name` (query, string): 
- `projectIds` (query, array): 

**Responses:**

- `200`: Ok → `IListProjectResponse`

---

### `DELETE /api/rest/v1/projects/{projectId}`

**DeleteProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/projects/{projectId}`

**GetProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `IProjectApi`

---

### `POST /api/rest/v1/projects/{projectId}`

**UpdateProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Request Body** (`application/json`): `IProjectUpdateBody`

**Responses:**

- `200`: Ok → `IProjectApi`

---

### `GET /api/rest/v1/projects/{projectId}/users`

**ListUsersForProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `IProjectUsersApiResponse`

---

### `POST /api/rest/v1/projects/{projectId}/add-users`

**AddUsersToProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Request Body** (`application/json`): `IProjectAddUsersBody`

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/projects/{projectId}/remove-users`

**RemoveUsersToProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Request Body** (`application/json`): `IProjectRemoveUsersBody`

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/projects/{projectId}/orchestrations`

**ListOrchestrations**

API to list all Orchestration assigned to given project.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `limit` (query, string): 
- `offset` (query, number): 
- `name` (query, string): 

**Responses:**

- `200`: Ok → `IListOrchestrationConfigResponse`

---

### `POST /api/rest/v1/projects/{projectId}/orchestrations`

**CreateOrchestration**

API to create new Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Request Body** (`application/json`): `IOrchestrationConfigCreateApi`

**Responses:**

- `200`: Ok → `IOrchestrationConfigApi`

---

### `DELETE /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}`

**DeleteOrchestration**

API to delete Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}`

**GetOrchestration**

API to get Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `IOrchestrationConfigApi`

---

### `PUT /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}`

**UpdatedOrchestration**

API to update Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 

**Request Body** (`application/json`): `IOrchestrationConfigUpdateApi`

**Responses:**

- `200`: Ok → `IOrchestrationConfigApi`

---

### `GET /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/run`

**RunOrchestration**

API to Run Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 
- `waitToFinish` (query, boolean): - default value is false, if true. Response wait until job is in final state.

**Responses:**

- `200`: Ok → `IOrchestrationRunApi`

---

### `GET /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/data-endpoint`

**DataEndpoint**

API to Run Orchestration for given inputs.

This Data Endpoint expects that given orchestration contains one or more InputAction with type `DATA_ENDPOINT`.

This API for valid inputs adds input data (or assembled them from additional query params) to orchestration context and run orchestration.

In case of DataEndpointModeEnum#QUERY

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 
- `mode` (query, string): 
- `waitToFinish` (query, boolean): - switch if call should wait to finish or not
- `waitUntil` (query, string): Use if you need to wait for specific action before you receive response.

Provide URL-encoded JSON object with following properties:

- actionOrderId - order number of action to wait for
- actionType - type of action to wait for within that action
- actionName - name of the specific action of type to wait for; leave empty for TRIGGER type

The action will report based on the exact step that will match given JSON object.
Eg. if the step within selected action fails, the request will throw 400 error and the response will contain the error message.
If any other action fails but this step does not fail, the request will return 200 response with orchestration details and orchestration state as "PROCESSING".
- `customSuccessResponse` (query, string): - if present custom endpoint return this JSON response for success - expects JSON object as url encoded string
- `customErrorResponse` (query, string): - if present custom endpoint return this JSON response for error - expects JSON object as url encoded string
- `data` (query, string): - optional param, expects ActionFlowData as url encoded string

**Responses:**

- `200`: Ok

---

### `POST /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/data-endpoint`

**DataEndpointPost**

API to Run Orchestration for given inputs.

This Data Endpoint expects that given orchestration contains one or more InputAction with type `DATA_ENDPOINT`.

This API for valid inputs adds input data to orchestration context and run orchestration.

In case of: conversionMode =    - body data are converted to `{ "root": [body] }`
In case of: conversionMode =    - body data are converted to `{ "root": body }`
In case of: conversionMode =    - body data are not modified

**Parameters:**

- `projectId` (path, string) *(required)*: - required param
- `orchestrationId` (path, string) *(required)*: - required param
- `conversionMode` (query, string): - optional param defines how to process input data to {@link ActionFlowData}, default value {@link DataEndpointPostModeEnum.AFDM}Available values:
- AFDM - take given object
- ARRAY - take given object and convert data from `[{ [key: string]: any }, ...]` to `{ "root": [{ [key: string]: any }, ...] }`
- SINGLE - take given object and convert data from `{ [key: string]: any }` to `{ "root": [{ [key: string]: any }] }`
- `waitToFinish` (query, boolean): - switch if call should wait to finish or not
- `waitUntil` (query, string): Use if you need to wait for specific action before you receive response.

Provide URL-encoded JSON object with following properties:

- actionOrderId - order number of action to wait for
- actionType - type of action to wait for within that action
- actionName - name of the specific action of type to wait for; leave empty for TRIGGER type

The action will report based on the exact step that will match given JSON object.
Eg. if the step within selected action fails, the request will throw 400 error and the response will contain the error message.
If any other action fails but this step does not fail, the request will return 200 response with orchestration details and orchestration state as "PROCESSING".
- `customSuccessResponse` (query, string): - if present custom endpoint return this JSON response for success - expects JSON object as url encoded string
- `customErrorResponse` (query, string): - if present custom endpoint return this JSON response for error - expects JSON object as url encoded string

**Request Body** (`application/json`):
```json
{
  "anyOf": [
    {
      "$ref": "#/components/schemas/ActionFlowData"
    },
    {
      "items": {
        "$ref": "#/components/schemas/ActionFlowDataItem"
      },
      "type": "array"
    },
    {
      "$ref": "#/components/schemas/ActionFlowDataItem"
    }
  ],
  "description": "- required param"
}
```

**Responses:**

- `200`: Ok

---

### `PUT /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/disable`

**DisableOrchestration**

API to disable Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `PUT /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/enable`

**EnableOrchestration**

API to enable Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `PUT /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/force-restart`

**ForceRestartOrchestration**

API to force restart Orchestration for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/runs`

**ListRuns**

API to list all Orchestration Runs assigned to given project.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 
- `limit` (query, string): 
- `offset` (query, number): 
- `state` (query, string): 

**Responses:**

- `200`: Ok → `IListOrchestrationRunResponse`

---

### `GET /api/rest/v1/projects/{projectId}/orchestrations/{orchestrationId}/runs/{runId}`

**GetRun**

API to get Orchestration Run for given inputs.

**Parameters:**

- `projectId` (path, string) *(required)*: 
- `orchestrationId` (path, string) *(required)*: 
- `runId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `IOrchestrationRunApi`

---

