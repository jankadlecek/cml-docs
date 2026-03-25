# ClickHouse Input Client

> **Context for AI:** Reads data from ClickHouse analytical databases.
> Supports parameterized queries.
>
> **See also:** [Shared input types](_types.md), [ClickHouse connection](../connections.md)

### `IQueryBindParam`

Defines a bind parameter used in a ClickHouse SQL query.

The parameter name must match the placeholder used in the SQL
statement (without braces).

Example:
```sql
WHERE run_id = {runId:String}
```

```json
{
  "description": "Defines a bind parameter used in a ClickHouse SQL query.\n\nThe parameter name must match the placeholder used in the SQL\nstatement (without braces).\n\nExample:\n```sql\nWHERE run_id = {runId:String}\n```",
  "properties": {
    "name": {
      "type": "string",
      "description": "Name of the bind parameter as referenced in the SQL query."
    },
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam",
      "description": "Context parameter definition used to resolve the runtime value."
    }
  },
  "required": [
    "name",
    "contextParam"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IClickHouseInputClient`

Only SELECT queries are supported. Any other SQL command
(INSERT, UPDATE, DELETE, DDL, etc.) will result in an ERROR.

Bind parameters are resolved from runtime context values and
passed to ClickHouse as query parameters.

```json
{
  "description": "Only SELECT queries are supported. Any other SQL command\n(INSERT, UPDATE, DELETE, DDL, etc.) will result in an ERROR.\n\nBind parameters are resolved from runtime context values and\npassed to ClickHouse as query parameters.",
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "authId": {
      "type": "string",
      "description": "Authentication identifier used to resolve a ClickHouse connection."
    },
    "bindParams": {
      "items": {
        "$ref": "#/components/schemas/IQueryBindParam"
      },
      "type": "array",
      "description": "Optional list of bind parameters to be injected into the SQL query.\n\nBind params are substituted using ClickHouse query parameters\nsyntax: `{paramName:Type}`.\n\nExample:\n```sql\nSELECT *\nFROM events\nWHERE run_id = {runId:String}\n  AND created_at >= parseDateTimeBestEffort({from:String})\n```"
    },
    "sqlString": {
      "type": "string",
      "description": "SQL SELECT statement to execute.\n\nThe query must be valid ClickHouse SQL and reference existing\ntables and columns."
    }
  },
  "required": [
    "sqlString"
  ],
  "type": "object",
  "additionalProperties": false
}
```

