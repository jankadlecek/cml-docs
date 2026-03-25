# Google BigQuery Output Client

> **Context for AI:** Loads data into Google BigQuery tables.
> Supports schema definition with column types.
>
> **See also:** [Shared output types](_types.md), [Google auth](../auth/auth-types.md)

### `BigQueryTableFieldSchema`

[Actual docs BQ Docs](https://cloud.google.com/bigquery/docs/reference/rest/v2/tables#tableschema)

```json
{
  "properties": {
    "scale": {
      "type": "string"
    },
    "precision": {
      "type": "string"
    },
    "maxLength": {
      "type": "string"
    },
    "policyTags": {
      "properties": {
        "names": {
          "items": {
            "type": "string"
          },
          "type": "array"
        }
      },
      "required": [
        "names"
      ],
      "type": "object"
    },
    "description": {
      "type": "string"
    },
    "fields": {
      "items": {
        "$ref": "#/components/schemas/BigQueryTableFieldSchema"
      },
      "type": "array"
    },
    "mode": {
      "type": "string"
    },
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
  "type": "object",
  "description": "[Actual docs BQ Docs](https://cloud.google.com/bigquery/docs/reference/rest/v2/tables#tableschema)"
}
```

### `IGoogleBQOutputClient`

```json
{
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "projectId": {
      "type": "string"
    },
    "datasetId": {
      "type": "string"
    },
    "tableId": {
      "type": "string"
    },
    "credentials": {
      "$ref": "#/components/schemas/GoogleCredentialsType",
      "description": "DEPRECATED\nUse authId instead.\n\nUsed to override default service account used to access given table."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding credentials."
    },
    "schemaFields": {
      "items": {
        "$ref": "#/components/schemas/BigQueryTableFieldSchema"
      },
      "type": "array"
    },
    "writeDisposition": {
      "type": "string",
      "description": "To fill this attribute please refer to [BG Docs](https://cloud.google.com/bigquery/docs/reference/rest/v2/Job#jobconfigurationload)"
    },
    "useStream": {
      "type": "boolean",
      "description": "This switch change inserting data from JSON load to stream insert. This should be used with caution. Please follow this rules:\n- Truncate table is not supported (only APPEND)\n- define table schema (in case the table not exists)\n- does not support special characters for column names (this will be fixed in future release)\n   - please refer to BQ Doc to set valid names\n\nCAUTION: THIS INSERT METHOD HAS ADDITIONAL COSTS. Please refer to: [BQ Pricing](https://cloud.google.com/bigquery/pricing#data_ingestion_pricing)"
    }
  },
  "required": [
    "datasetId",
    "tableId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Pick_IGoogleBQOutputClient.Exclude_keyofIGoogleBQOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "authId": {
      "type": "string",
      "description": "ID to auth component holding credentials."
    },
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "credentials": {
      "$ref": "#/components/schemas/GoogleCredentialsType",
      "description": "DEPRECATED\nUse authId instead.\n\nUsed to override default service account used to access given table."
    },
    "projectId": {
      "type": "string"
    },
    "datasetId": {
      "type": "string"
    },
    "tableId": {
      "type": "string"
    },
    "schemaFields": {
      "items": {
        "$ref": "#/components/schemas/BigQueryTableFieldSchema"
      },
      "type": "array"
    },
    "writeDisposition": {
      "type": "string",
      "description": "To fill this attribute please refer to [BG Docs](https://cloud.google.com/bigquery/docs/reference/rest/v2/Job#jobconfigurationload)"
    },
    "useStream": {
      "type": "boolean",
      "description": "This switch change inserting data from JSON load to stream insert. This should be used with caution. Please follow this rules:\n- Truncate table is not supported (only APPEND)\n- define table schema (in case the table not exists)\n- does not support special characters for column names (this will be fixed in future release)\n   - please refer to BQ Doc to set valid names\n\nCAUTION: THIS INSERT METHOD HAS ADDITIONAL COSTS. Please refer to: [BQ Pricing](https://cloud.google.com/bigquery/pricing#data_ingestion_pricing)"
    }
  },
  "required": [
    "datasetId",
    "tableId"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_IGoogleBQOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_IGoogleBQOutputClient.Exclude_keyofIGoogleBQOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

