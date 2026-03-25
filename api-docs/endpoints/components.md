# Components API (Tokens & Auths)

> **Context for AI:** Components manage authentication credentials and access tokens in CML.
> - **Tokens**: Generate access tokens for API authentication, including embed tokens for embedded UIs
> - **Auths**: Store and manage authentication configurations for external services (APIs, databases, FTP, etc.)
>
> **Related schemas:** [tokens](../schemas/tokens.md), [auth types](../schemas/auth/auth-types.md)

## Endpoints

### `POST /api/rest/v1/components/tokens`

**CreateToken**

**Request Body** (`application/json`): `ITokenCreate`

**Responses:**

- `200`: Ok → `ITokenCreateResponse`

---

### `GET /api/rest/v1/components/tokens/{tokenId}/generate`

**TokenGenerate**

**Parameters:**

- `tokenId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `ITokenGenerateResponse`

---

### `POST /api/rest/v1/components/tokens/generateEmbedToken`

**GenerateEmbedToken**

**Request Body** (`application/json`): `IGenerateEmbedTokenApi`

**Responses:**

- `200`: Ok → `IGenerateEmbedTokenApiResponse`

---

### `POST /api/rest/v1/components/tokens/generate-multi-embed-token`

**GenerateMultiEmbedToken**

**Request Body** (`application/json`): `IGenerateMultiEmbedTokenApi`

**Responses:**

- `200`: Ok → `IGenerateEmbedTokenApiResponse`

---

### `POST /api/rest/v1/components/auths`

**CreateAuthorization**

**Request Body** (`application/json`): `AuthCreate`

**Responses:**

- `200`: Ok → `Auth`

---

### `GET /api/rest/v1/components/auths`

**ListAuthorizations**

**Parameters:**

- `name` (query, string): 
- `state` (query, string): 
- `type` (query, string): 

**Responses:**

- `200`: Ok

---

### `GET /api/rest/v1/components/auths/{authId}`

**GetAuthorization**

**Parameters:**

- `authId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `Auth`

---

### `DELETE /api/rest/v1/components/auths/{authId}`

**DeleteAuthorization**

**Parameters:**

- `authId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/components/auths/{authId}/send-auth-link`

**GenerateAuthLink**

**Parameters:**

- `authId` (path, string) *(required)*: 
- `emailRecipient` (query, string) *(required)*: 

**Responses:**

- `200`: Ok

---

