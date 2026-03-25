# Google BigQuery Input Client

> **Context for AI:** Reads data from Google BigQuery using SQL queries.
> Supports parameterized queries with bind params for safe value injection.
>
> **See also:** [Shared input types](_types.md), [Google auth](../auth/auth-types.md)

### `IGBQBindParams`

Contains all bind params to be used in query. In BQ select use

```json
{
  "description": "Contains all bind params to be used in query. In BQ select use",
  "properties": {},
  "type": "object",
  "additionalProperties": {}
}
```

### `IGBQBindContextParam`

Contains all bind context params to be used in query. In BQ select use

```json
{
  "description": "Contains all bind context params to be used in query. In BQ select use",
  "properties": {
    "name": {
      "type": "string",
      "description": "Contains name of the bind param. Used in query"
    },
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam",
      "description": "Contains definition how to bind context param"
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

### `IQueryGBQInputClient`

```json
{
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "projectId": {
      "type": "string",
      "description": "Used to override default project."
    },
    "datasetId": {
      "type": "string",
      "description": "Defines which dataset in Big Query to bee used for querying."
    },
    "tableId": {
      "type": "string",
      "description": "Defines which table in Big Query to bee used for querying."
    },
    "credentials": {
      "$ref": "#/components/schemas/GoogleCredentialsType",
      "description": "DEPRECATED\nUse authId instead.\n\nUsed to override default service account used to access given table."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding credentials."
    },
    "query": {
      "type": "string",
      "description": "Contains the slq statement to be used to query data."
    },
    "bindParams": {
      "$ref": "#/components/schemas/IGBQBindParams",
      "description": "Possibility to use specific bind params. E.g. runId, now, and others"
    },
    "bindContextParams": {
      "items": {
        "$ref": "#/components/schemas/IGBQBindContextParam"
      },
      "type": "array",
      "description": "Possibility to use context bind params. E.g. runId, now, and others"
    }
  },
  "required": [
    "datasetId",
    "tableId",
    "query"
  ],
  "type": "object",
  "additionalProperties": false
}
```

