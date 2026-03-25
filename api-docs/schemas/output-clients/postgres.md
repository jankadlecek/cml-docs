# PostgreSQL Output Client

> **Context for AI:** Loads data into PostgreSQL tables.
> Supports multiple write modes and column definitions.
>
> **See also:** [Shared output types](_types.md), [Postgres connection](../connections.md)

### `PgWriteMode`

```json
{
  "enum": [
    "APPEND",
    "TRUNCATE",
    "REPLACE",
    "UPDATE",
    "MERGE"
  ],
  "type": "string"
}
```

### `IPgColumnDefinition`

```json
{
  "properties": {
    "name": {
      "type": "string",
      "pattern": "^[a-zA-Z_][a-zA-Z0-9_]*$",
      "minLength": 1
    },
    "type": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "string",
          "enum": [
            "TEXT",
            "VARCHAR",
            "INTEGER",
            "BIGINT",
            "DECIMAL",
            "NUMERIC",
            "FLOAT",
            "DOUBLE",
            "BOOLEAN",
            "DATE",
            "TIMESTAMP",
            "TIMESTAMPTZ",
            "JSON",
            "JSONB",
            "UUID",
            "ARRAY"
          ]
        }
      ]
    },
    "nullable": {
      "type": "boolean"
    },
    "defaultValue": {
      "type": "string"
    },
    "primaryKey": {
      "type": "boolean"
    }
  },
  "required": [
    "name",
    "type"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IPgOutputClient`

```json
{
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "authId": {
      "type": "string"
    },
    "tableName": {
      "type": "string",
      "pattern": "^[a-zA-Z_][a-zA-Z0-9_]*$",
      "minLength": 1
    },
    "tableSchema": {
      "type": "string",
      "pattern": "^[a-zA-Z_][a-zA-Z0-9_]*$",
      "minLength": 1
    },
    "writeMode": {
      "$ref": "#/components/schemas/PgWriteMode"
    },
    "columnSchema": {
      "items": {
        "$ref": "#/components/schemas/IPgColumnDefinition"
      },
      "type": "array"
    }
  },
  "required": [
    "authId",
    "tableName",
    "tableSchema",
    "writeMode"
  ],
  "type": "object",
  "additionalProperties": false
}
```

