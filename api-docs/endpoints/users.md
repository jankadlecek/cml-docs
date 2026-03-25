# Users API

> **Context for AI:** This file contains endpoints for user authentication and management in CML.
> Users can be of type SERVICE (API accounts) or USER (human accounts).
> Each user belongs to an organization and has scopes defining their permissions.
>
> **Related schemas:** [users](../schemas/users.md), [auth credentials](../schemas/auth/credentials.md)

## Endpoints

### `POST /api/rest/v1/users/authenticate`

**Authenticate**

**Request Body** (`application/json`): `ICredentialsApi`

**Responses:**

- `200`: Ok → `IUserAuthorizationTokenApi`

---

### `GET /api/rest/v1/users/me`

**GetMe**

**Responses:**

- `200`: Ok → `IUserResponseApi`

---

### `POST /api/rest/v1/users/me/change-password`

**ChangePassword**

**Request Body** (`application/json`):
```json
{
  "properties": {
    "password": {
      "type": "string"
    }
  },
  "required": [
    "password"
  ],
  "type": "object"
}
```

**Responses:**

- `204`: No content

---

### `POST /api/rest/v1/users`

**Create**

**Request Body** (`application/json`): `IUserCreateApi`

**Responses:**

- `200`: Ok

---

### `PUT /api/rest/v1/users/{userId}/enable`

**Enable**

**Parameters:**

- `userId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `PUT /api/rest/v1/users/{userId}/disable`

**Disable**

**Parameters:**

- `userId` (path, string) *(required)*: 

**Responses:**

- `204`: No content

---

### `GET /api/rest/v1/projects/{projectId}/users`

**ListUsersForProject**

**Parameters:**

- `projectId` (path, string) *(required)*: 

**Responses:**

- `200`: Ok → `IProjectUsersApiResponse`

---

