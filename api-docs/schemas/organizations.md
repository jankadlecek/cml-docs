# Organization Schemas

> **Context for AI:** Organizations are the top-level entity. Each user and project belongs
> to exactly one organization.
>
> **See also:** [Organizations endpoints](../endpoints/organizations.md)

### `IOrganizationApi`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "projects": {
      "items": {
        "$ref": "#/components/schemas/IProjectApi"
      },
      "type": "array"
    },
    "users": {
      "items": {
        "$ref": "#/components/schemas/IOrganizationUser"
      },
      "type": "array"
    }
  },
  "required": [
    "id",
    "projects",
    "users"
  ],
  "type": "object",
  "additionalProperties": false
}
```

