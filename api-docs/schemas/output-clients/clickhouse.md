# ClickHouse Output Client

> **Context for AI:** Loads data into ClickHouse tables.
> Supports table engine configuration and custom clauses.
>
> **See also:** [Shared output types](_types.md), [ClickHouse connection](../connections.md)

### `CHWriteMode`

ClickHouse write mode.

```json
{
  "type": "string",
  "enum": [
    "APPEND",
    "TRUNCATE"
  ],
  "description": "ClickHouse write mode."
}
```

### `IClickHouseEngineInput`

ClickHouse table engine configuration.

If omitted, `MergeTree` is used by default.
Engine arguments and expressions are passed verbatim to ClickHouse.

Example
```
{
  "expression": "ReplacingMergeTree",
  "arguments": ["version"]
}
```

```json
{
  "description": "ClickHouse table engine configuration.\n\nIf omitted, `MergeTree` is used by default.\nEngine arguments and expressions are passed verbatim to ClickHouse.\n\nExample\n```\n{\n  \"expression\": \"ReplacingMergeTree\",\n  \"arguments\": [\"version\"]\n}\n```",
  "properties": {
    "expression": {
      "type": "string"
    },
    "arguments": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "expression"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IClickHouseClauseInput`

Generic ClickHouse clause input.

Used for:
- ORDER BY
- PARTITION BY
- engine expressions

Expressions and arguments are rendered verbatim.

```json
{
  "description": "Generic ClickHouse clause input.\n\nUsed for:\n- ORDER BY\n- PARTITION BY\n- engine expressions\n\nExpressions and arguments are rendered verbatim.",
  "properties": {
    "expression": {
      "type": "string"
    },
    "arguments": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "expression"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IClickHouseOutputClient`

ClickHouse output client configuration.

This client performs batched INSERT operations into ClickHouse tables.
Table creation is optional and depends on whether `tableSchema` is provided.
Notes:
- INSERTs are asynchronous; data may not be immediately visible.
- When `tableSchema` is omitted, ClickHouse applies default values
  for missing columns and ignores extra fields.
- No column input validation is performed unless `tableSchema` is provided.

Valid schema and engine configuration are expected.

```json
{
  "description": "ClickHouse output client configuration.\n\nThis client performs batched INSERT operations into ClickHouse tables.\nTable creation is optional and depends on whether `tableSchema` is provided.\nNotes:\n- INSERTs are asynchronous; data may not be immediately visible.\n- When `tableSchema` is omitted, ClickHouse applies default values\n  for missing columns and ignores extra fields.\n- No column input validation is performed unless `tableSchema` is provided.\n\nValid schema and engine configuration are expected.",
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "authId": {
      "type": "string",
      "description": "Authentication identifier used to resolve a ClickHouse connection."
    },
    "tableName": {
      "type": "string",
      "description": "Target ClickHouse table name.",
      "pattern": "^[a-zA-Z_][a-zA-Z0-9_]*$",
      "minLength": 1
    },
    "tableSchema": {
      "items": {
        "properties": {
          "type": {
            "type": "string"
          },
          "name": {
            "type": "string"
          }
        },
        "required": [
          "type",
          "name"
        ],
        "type": "object"
      },
      "type": "array",
      "description": "List of column names defining the table schema.\nData inserted must match these column names.\n\nExample\n```\n[\n  { \"name\": \"id\", \"type\": \"String\" },\n  { \"name\": \"tenant_id\", \"type\": \"String\" },\n  { \"name\": \"created_at\", \"type\": \"DateTime\" }\n]\n```"
    },
    "writeMode": {
      "$ref": "#/components/schemas/CHWriteMode",
      "description": "Write mode for the operation.\n\n- `APPEND` \u2014 insert data into the existing table\n- `TRUNCATE` \u2014 truncate the table before inserting data"
    },
    "engine": {
      "$ref": "#/components/schemas/IClickHouseEngineInput",
      "description": "ClickHouse table engine configuration.\n\nIf omitted, `MergeTree` is used by default.\nEngine arguments and expressions are passed verbatim to ClickHouse.\n\nExample\n```\n{\n  \"expression\": \"ReplacingMergeTree\",\n  \"arguments\": [\"version\"]\n}\n```"
    },
    "partition": {
      "items": {
        "$ref": "#/components/schemas/IClickHouseClauseInput"
      },
      "type": "array",
      "description": "PARTITION BY configuration.\n\nSupports one or multiple partition expressions.\nExpressions are rendered into a single `PARTITION BY` clause.\n\nExample\n```\n[\n  { \"expression\": \"toYYYYMM\", \"arguments\": [\"created_at\"] }\n]\n\n```\nResults in\nPARTITION BY toYYYYMM(created_at)\n```"
    },
    "orderBy": {
      "items": {
        "$ref": "#/components/schemas/IClickHouseClauseInput"
      },
      "type": "array",
      "description": "ORDER BY clause used for MergeTree engines.\n\nColumn references are validated ONLY when `tableSchema` is provided.\n\nExample\n```\n[{ expression: \"tuple\"; columns: [\"id\", \"tenant_id\"] }] results in \"ORDER BY (tuple(id, tenant_id))\"\n```\n\nExample\n```\n[{ expression: \"tenant_id\" }]\nResults in\n\"ORDER BY tenant_id\"\n```"
    }
  },
  "required": [
    "authId",
    "tableName",
    "writeMode"
  ],
  "type": "object",
  "additionalProperties": false
}
```

