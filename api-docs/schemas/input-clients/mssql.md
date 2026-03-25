# MSSQL Input Client

> **Context for AI:** Reads data from Microsoft SQL Server databases.
> Supports parameterized queries.
>
> **See also:** [Shared input types](_types.md), [MSSQL connection](../connections.md)

### `IMssqlBindParam`

Defines Bind params for MS SQL.
Example:
```
{
 "order": 1,
    "contextParam": {
        "name": "now"
    }
}
```

```json
{
  "description": "Defines Bind params for MS SQL.\nExample:\n```\n{\n \"order\": 1,\n    \"contextParam\": {\n        \"name\": \"now\"\n    }\n}\n```",
  "properties": {
    "order": {
      "type": "integer",
      "format": "int32",
      "description": "Provides order for given bind params. This number has to be equal with number in the select.\nExample:\nselect = \"select * from where last_modified >",
      "example": "1",
      "minimum": 1
    },
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam",
      "description": "Defines which context param should be used for binding."
    }
  },
  "required": [
    "order",
    "contextParam"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IMssqlInputClient`

MS SQL Input Client object definition. This input client is used to get data from MS SQL Server.

```json
{
  "description": "MS SQL Input Client object definition. This input client is used to get data from MS SQL Server.",
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "bindParams": {
      "items": {
        "$ref": "#/components/schemas/IMssqlBindParam"
      },
      "type": "array",
      "description": "Provides possibility to add bind params.\nExample:\n[\n {\n   \"order\": 1,\n      \"contextParam\": {\n          \"name\": \"now\"\n      }\n },\n {\n    \"order\": 2,\n      \"contextParam\": {\n          \"name\": \"prevStart\"\n      }\n }\n]"
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding connection details.\nRequired for MSSQL connection."
    },
    "sqlString": {
      "type": "string",
      "description": "The sql statement to be executed.\nUse"
    }
  },
  "required": [
    "authId",
    "sqlString"
  ],
  "type": "object",
  "additionalProperties": false
}
```

