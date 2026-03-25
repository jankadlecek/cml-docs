# Project Schemas

> **Context for AI:** Projects are the main organizational unit containing orchestrations
> and transformation engines. Each project has assigned users with project-level scopes.
>
> **See also:** [Projects endpoints](../endpoints/projects.md)

### `IProjectApi`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "createdAt": {
      "type": "string",
      "format": "date-time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time"
    },
    "name": {
      "type": "string",
      "maxLength": 100
    },
    "organizationId": {
      "type": "string",
      "maxLength": 100
    },
    "users": {
      "items": {
        "$ref": "#/components/schemas/IProjectUserApi"
      },
      "type": "array"
    }
  },
  "required": [
    "id",
    "createdAt",
    "updatedAt",
    "name",
    "organizationId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectCreateBody`

```json
{
  "properties": {
    "name": {
      "type": "string",
      "maxLength": 100
    }
  },
  "required": [
    "name"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IListProjectResponse`

```json
{
  "properties": {
    "count": {
      "type": "integer",
      "format": "int32"
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/IProjectApi"
      },
      "type": "array",
      "description": "Loaded date for given type"
    },
    "pageInfo": {
      "$ref": "#/components/schemas/IPageInfo",
      "description": "Current page info."
    }
  },
  "required": [
    "count",
    "data",
    "pageInfo"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProjectUpdateBody`

```json
{
  "properties": {
    "name": {
      "type": "string",
      "maxLength": 100
    }
  },
  "required": [
    "name"
  ],
  "type": "object",
  "additionalProperties": false
}
```

