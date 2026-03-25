# User Schemas

> **Context for AI:** User-related data types for CML user management.
> Users can be assigned to projects with specific scopes.
> Two account types exist: USER (human) and SERVICE (API/automation).
>
> **See also:** [Users endpoints](../endpoints/users.md), [Projects](projects.md)

### `IUserResponseApi`

```json
{
  "properties": {
    "id": {
      "type": "string",
      "description": "Resource's unique ID"
    },
    "createdAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s creation date with time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s last update date with time"
    },
    "userName": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "surname": {
      "type": "string"
    },
    "type": {
      "$ref": "#/components/schemas/AccountTypeEnum"
    },
    "organizationId": {
      "type": "string"
    },
    "scopes": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "accessLimit": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "id",
    "createdAt",
    "updatedAt",
    "userName",
    "type",
    "organizationId",
    "scopes",
    "accessLimit"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IUserCreateApi`

```json
{
  "properties": {
    "userName": {
      "type": "string",
      "description": "New user's userName",
      "example": "tester"
    },
    "name": {
      "type": "string",
      "nullable": true,
      "description": "New user's name - not used for SERVICE ACCOUNTS",
      "example": "Tester"
    },
    "surname": {
      "type": "string",
      "nullable": true,
      "description": "New user's surname - not used for SERVICE ACCOUNTS",
      "example": "Tester"
    },
    "password": {
      "type": "string",
      "description": "New user's password",
      "example": "mySuperStrongPassword"
    },
    "type": {
      "$ref": "#/components/schemas/AccountTypeEnum",
      "description": "New user's account type",
      "example": "SERVICE"
    },
    "organizationId": {
      "type": "string",
      "description": "New user's organization",
      "example": "marketing_bi"
    },
    "scopes": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "description": "Scopes. Used to define access for given API.\nProvides scopes related to the whole system.\nTo define project related scopes please refer to the API {@link \"projects/{projectId}/add-users\"}"
    },
    "accessLimit": {
      "type": "integer",
      "format": "int32",
      "nullable": true,
      "description": "Used to set token expiration in hours.",
      "example": "1"
    }
  },
  "required": [
    "userName",
    "password",
    "type",
    "organizationId",
    "scopes",
    "accessLimit"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectUserApi`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "userName": {
      "type": "string"
    }
  },
  "required": [
    "id",
    "userName"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectUserApiResponse`

```json
{
  "properties": {
    "userId": {
      "type": "string"
    },
    "scopes": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "userId",
    "scopes"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectUsersApiResponse`

```json
{
  "properties": {
    "projectId": {
      "type": "string"
    },
    "users": {
      "items": {
        "$ref": "#/components/schemas/IProjectUserApiResponse"
      },
      "type": "array"
    }
  },
  "required": [
    "projectId",
    "users"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectAddUsersBody`

```json
{
  "properties": {
    "users": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "minItems": 1
    },
    "scopes": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "minItems": 1
    }
  },
  "required": [
    "users",
    "scopes"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectRemoveUsersBody`

```json
{
  "properties": {
    "users": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "minItems": 1
    }
  },
  "required": [
    "users"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IOrganizationUser`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "surname": {
      "type": "string"
    },
    "userName": {
      "type": "string"
    }
  },
  "required": [
    "id",
    "name",
    "surname",
    "userName"
  ],
  "type": "object",
  "additionalProperties": false
}
```

