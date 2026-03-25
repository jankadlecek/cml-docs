# Playground API

> **Context for AI:** The Playground provides testing endpoints for CML actions.
> Developers can test input, output, and transformation actions in isolation
> without creating full orchestrations. It also includes mock data endpoints
> for testing (orders API) and utility endpoints (token generation, output sink).
>
> **Related schemas:** [actions](../schemas/actions/), [playground mock](../schemas/playground-mock.md)

## Endpoints

### `POST /api/rest/v1/playground/actions/run-input-action`

**RunInputAction**

API to run input action for given inputs.

**Request Body** (`application/json`):
```json
{
  "anyOf": [
    {
      "$ref": "#/components/schemas/IPlaygroundInputActionBody"
    },
    {
      "$ref": "#/components/schemas/IInputActionConfigApi"
    }
  ]
}
```

**Responses:**

- `200`: Ok → `InputActionResultApi`

---

### `POST /api/rest/v1/playground/actions/run-output-action`

**RunOutputAction**

API to run input action for given inputs.

**Request Body** (`application/json`): `IOutputActionBody`

**Responses:**

- `200`: Ok → `OutputActionResultApi`

---

### `POST /api/rest/v1/playground/actions/run-transformation-action`

**RunTransformationAction**

API to run input action for given inputs.

**Request Body** (`application/json`): `ITransformationActionBody`

**Responses:**

- `200`: Ok → `ITransformationActionResultApi`

---

### `POST /api/rest/v1/playground/actions/run-action`

**RunAction**

API to run action for given inputs.

**Request Body** (`application/json`): `IActionBody`

**Responses:**

- `200`: Ok → `GenericResultType`

---

### `GET /api/rest/v1/playground/mock/orders`

**MockOrderList**

API to get list of orders.

**Parameters:**

- `limit` (query, string): 
- `offset` (query, number): 

**Responses:**

- `200`: Ok → `IListMockDataOrderPage`

---

### `GET /api/rest/v1/playground/mock/orders/{orderId}`

**MockOrderDetail**

API to get order detail.

**Parameters:**

- `orderId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `MockDataOrder`

---

### `POST /api/rest/v1/playground/generate-token`

**GenerateToken**

API to generate test JWT token for given inputs.

**Request Body** (`application/json`): `IMockToken`

**Responses:**

- `200`: Ok → `ITokenGenerateResponse`

---

### `POST /api/rest/v1/playground/output-sink`

**OutputSinkPost**

Output sink endpoint supporting POST, PUT, and DELETE methods.
Accepts any optional request body and returns the specified status code.

**Parameters:**

- `statusCode` (query, string): The HTTP status code to respond with.

**Request Body** (`application/json`):
```json
{
  "description": "Optional request body. The body is always returned back."
}
```

**Responses:**

- `200`: Ok

---

### `PUT /api/rest/v1/playground/output-sink`

**OutputSinkPut**

**Parameters:**

- `statusCode` (query, string): 

**Request Body** (`application/json`):
```json
{}
```

**Responses:**

- `200`: Ok

---

### `DELETE /api/rest/v1/playground/output-sink`

**OutputSinkDelete**

**Parameters:**

- `statusCode` (query, string): 

**Request Body** (`application/json`):
```json
{}
```

**Responses:**

- `200`: Ok

---

### `POST /api/rest/v1/playground/basic-auth/token`

**GenerateTokenWithBasicAuth**

Basic auth token generation endpoint. The client is expected to send a base64 encoded (username:password) Authorization: Basic header.
Only checks that basic auth challange was passed.
Generates a random token and sets expiration headers.
The generated token is practically useless.

**Request Body** (`application/json`): `ICredentialsApi`

**Responses:**

- `200`: An object containing the generated token.

---

### `GET /api/rest/v1/playground/mock/socket-hang-up`

**MockSocketHangUp**

API to simulate 'socket hang up' error.
It is intended to be used in manual and automated testing.

**Parameters:**

- `param1` (query, number): 
- `param2` (query, number): 

**Responses:**

- `204`: No content

---

