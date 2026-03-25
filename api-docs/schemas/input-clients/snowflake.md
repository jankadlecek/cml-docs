# Snowflake Input Client

> **Context for AI:** Reads data from Snowflake data warehouse using SQL queries.
> Supports bind parameters for safe query parameterization.
>
> **See also:** [Shared input types](_types.md), [Snowflake connection](../connections.md)

### `ISnowflakeBindParam`

Defines Bind params for Snowflake.
Example:
```
{
    "order": 1,
    "contextParam": {
        "name": "prevStart"
    }
}
```

```json
{
  "description": "Defines Bind params for Snowflake.\nExample:\n```\n{\n    \"order\": 1,\n    \"contextParam\": {\n        \"name\": \"prevStart\"\n    }\n}\n```",
  "properties": {
    "order": {
      "type": "integer",
      "format": "int32",
      "description": "Provides order for given bind params. This number has to be equal with number in the select.\nExample:\n```\n select = \"select * from where last_modified > :1\"\n order = 1\n```",
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

### `ISnowflakeInputClient`

Snowflake Input Client object definition. This input client is used to get data from Snowflake.

```json
{
  "description": "Snowflake Input Client object definition. This input client is used to get data from Snowflake.",
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "bindParams": {
      "items": {
        "$ref": "#/components/schemas/ISnowflakeBindParam"
      },
      "type": "array",
      "description": "Provides possibility to add bind params.\nExample:\n```\n [\n     {\n         \"order\": 1,\n         \"contextParam\": {\n             \"name\": \"now\"\n             }\n     },{\n         \"order\": 2,\n         \"contextParam\": {\n             \"name\": \"prevStart\"\n             }\n     }\n ]\n```",
      "example": "some-uu-id-v4"
    },
    "connection": {
      "$ref": "#/components/schemas/ISnowflakeConnection",
      "description": "DEPRECATED\nUse authId instead.\n\nProvides connection detail to Snowflake. Overrides authId if exists."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding connection details.\nIs required if connection object is not provided."
    },
    "sqlString": {
      "type": "string",
      "description": "The sql statement to be executed.",
      "example": "select * from my_test_table where id in (1, 2)"
    }
  },
  "required": [
    "sqlString"
  ],
  "type": "object",
  "additionalProperties": false
}
```

