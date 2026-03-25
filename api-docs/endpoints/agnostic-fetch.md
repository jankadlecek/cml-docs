# Agnostic Fetch API

> **Context for AI:** Agnostic Fetch handles integration with external e-commerce platforms.
> Currently supports Shoptet (Czech e-commerce platform) with OAuth flow,
> suspend/reactivate lifecycle management, and data refresh capabilities.
>
> **Related schemas:** [agnostic-fetch](../schemas/agnostic-fetch.md)

## Endpoints

### `GET /api/rest/v1/agnostic-fetch/shoptet/oauth`

**AgnosticFetchShoptetOauth**

Handles the Shoptet OAuth flow and initializes the integration.

This endpoint:
1. Retrieves OAuth access token from Shoptet using the provided authorization code
2. Obtains user email from Shoptet using the access token
3. Sets up the integration by:
   - Creating or reactivating company in Datalook
   - Creating necessary service accounts
   - Setting up project orchestration
   - Generating and running full-refresh template

The endpoint must return 200 OK within 5 seconds as required by Shoptet.

**Parameters:**

- `code` (query, string) *(required)*: - OAuth authorization code from Shoptet

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/agnostic-fetch/shoptet/suspend`

**AgnosticFetchShoptetSuspend**

Suspends the Shoptet integration for a specific eshop.

This endpoint:
1. Finds the company by external ID
2. Deactivates the company and all its orchestrations

**Request Body** (`application/json`):
```json
{
  "description": "- Notification body from Shoptet"
}
```

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/agnostic-fetch/shoptet/reactivate`

**AgnosticFetchShoptetReactivate**

Reactivates the Shoptet integration for a specific eshop.

This endpoint:
1. Finds the company by external ID
2. Reactivates the company and all its orchestrations

**Request Body** (`application/json`):
```json
{
  "description": "- Notification body from Shoptet"
}
```

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/agnostic-fetch/shoptet/remove`

**AgnosticFetchShoptetRemove**

Removes the Shoptet integration for a specific eshop.

This endpoint:
1. Finds the company by external ID
2. Deletes auth component for the company
3. Deactivates the company and all its orchestrations

**Request Body** (`application/json`):
```json
{
  "description": "- Notification body from Shoptet"
}
```

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/agnostic-fetch/{projectId}/refresh`

**AgnosticFetchRefresh**

Refreshes the Shoptet integration for a specific eshop.

This endpoint:
1. Deactivates previous orchestration (if any).
2. Creates and triggers new orchestration with incremental refresh template.

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Request Body** (`application/json`): `RefreshAgnosticFetchInput`

**Responses:**

- `204`: No content

---

