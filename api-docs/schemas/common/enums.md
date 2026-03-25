# Common Enums

> **Context for AI:** Shared enumeration types used across the entire CML API.
> These enums define allowed values for various fields throughout the system.
> Import these when you need to understand valid values for status fields,
> type discriminators, or configuration options.

### `AccountTypeEnum`

```json
{
  "enum": [
    "SERVICE",
    "USER"
  ],
  "type": "string"
}
```

### `LimitEnum`

```json
{
  "enum": [
    25,
    50,
    100,
    200
  ],
  "type": "number"
}
```

### `TokenTypeEnum`

```json
{
  "enum": [
    "GOOGLE_DATA_STUDIO"
  ],
  "type": "string"
}
```

### `AuthStateEnum`

```json
{
  "enum": [
    "SUCCESS",
    "PENDING",
    "ERROR"
  ],
  "type": "string"
}
```

### `AuthTypeEnum`

```json
{
  "enum": [
    "FACEBOOK",
    "GOOGLE_OAUTH2",
    "BASIC_AUTH_TO_BEARER",
    "AZURE_SERVICE_PRINCIPAL",
    "API_KEY",
    "CREDENTIALS"
  ],
  "type": "string"
}
```

### `EntityTypeEnum`

```json
{
  "enum": [
    "ORCHESTRATION",
    "PROJECT"
  ],
  "type": "string"
}
```

### `ActionFlowDataItemType`

```json
{
  "$ref": "#/components/schemas/Record_string.any_"
}
```

### `ActionFlowDataArrayType`

```json
{
  "items": {
    "$ref": "#/components/schemas/ActionFlowDataItemType"
  },
  "type": "array"
}
```

### `ActionFlowDataType`

```json
{
  "$ref": "#/components/schemas/Record_string.ActionFlowDataArrayType_"
}
```

### `GenericResultStatusEnum`

```json
{
  "enum": [
    "OK",
    "ERROR",
    "UNKNOWN"
  ],
  "type": "string"
}
```

### `DynamicParamTypeEnum`

```json
{
  "enum": [
    "BODY",
    "QUERY",
    "PATH",
    "HEADER"
  ],
  "type": "string"
}
```

### `ContextParamsEnum`

```json
{
  "enum": [
    "prevStart",
    "now",
    "runId",
    "parent"
  ],
  "type": "string"
}
```

### `ContextParamTypeEnum`

```json
{
  "enum": [
    "DATE",
    "CALCULATE_DATE"
  ],
  "type": "string"
}
```

### `ValidationSchemaObjectTypeEnum`

```json
{
  "enum": [
    "array",
    "object",
    "string",
    "number",
    "boolean"
  ],
  "type": "string"
}
```

### `ResultHookTypeEnum`

```json
{
  "enum": [
    "SUCCESS",
    "ERROR"
  ],
  "type": "string"
}
```

### `ExtractionConfigTypeEnum`

```json
{
  "enum": [
    "JSON",
    "XML"
  ],
  "type": "string"
}
```

### `DynamicParamTypeWithoutBody`

```json
{
  "$ref": "#/components/schemas/DynamicParamType"
}
```

### `MappingTypeEnum`

```json
{
  "enum": [
    "ONE",
    "MANY",
    "NONE"
  ],
  "type": "string"
}
```

### `ResultHookStatusEnum`

```json
{
  "enum": [
    "OK",
    "ERROR",
    "SKIPPED"
  ],
  "type": "string"
}
```

### `GenericProcessingStatusEnum`

```json
{
  "enum": [
    "PENDING",
    "PROCESSING"
  ],
  "type": "string"
}
```

### `GenericResultType`

```json
{
  "properties": {
    "executionTime": {
      "type": "number",
      "format": "double"
    },
    "childrenResults": {
      "items": {
        "$ref": "#/components/schemas/GenericResultType"
      },
      "type": "array"
    },
    "data": {},
    "status": {
      "$ref": "#/components/schemas/GenericResultStatusEnum"
    },
    "monitoringLogs": {
      "$ref": "#/components/schemas/IActionStatus"
    },
    "actionsLog": {
      "items": {
        "$ref": "#/components/schemas/IGenericActionResult"
      },
      "type": "array"
    }
  },
  "required": [
    "data",
    "status",
    "monitoringLogs",
    "actionsLog"
  ],
  "type": "object"
}
```

### `HttpStatusCodeLiteral`

```json
{
  "type": "number",
  "enum": [
    100,
    101,
    102,
    200,
    201,
    202,
    203,
    204,
    205,
    206,
    207,
    208,
    226,
    300,
    301,
    302,
    303,
    304,
    305,
    307,
    308,
    400,
    401,
    402,
    403,
    404,
    405,
    406,
    407,
    408,
    409,
    410,
    411,
    412,
    413,
    414,
    415,
    416,
    417,
    418,
    422,
    423,
    424,
    426,
    428,
    429,
    431,
    451,
    500,
    501,
    502,
    503,
    504,
    505,
    506,
    507,
    508,
    510,
    511
  ]
}
```

### `JobStateEnum`

```json
{
  "enum": [
    "DONE",
    "PROCESSING",
    "ERROR",
    "WARNING",
    "KILLED"
  ],
  "type": "string"
}
```

### `JobConfigStateEnum`

Enums

```json
{
  "description": "Enums",
  "enum": [
    "ACTIVE",
    "ERROR",
    "DISABLED"
  ],
  "type": "string"
}
```

### `DataEndpointModeEnum`

```json
{
  "enum": [
    "QUERY",
    "DATA"
  ],
  "type": "string"
}
```

### `DataEndpointPostModeEnum`

Contains specified values to switch dataEndpointPost handling input object converted to {@link ActionFlowData}.
Available values:
  - AFDM - take given object
  - ARRAY - take given object and convert data from `[{ [key: string]: any }, ...]` to `{ "root": [{ [key: string]: any }, ...] }`
  - SINGLE - take given object and convert data from `{ [key: string]: any }` to `{ "root": [{ [key: string]: any }] }`

```json
{
  "description": "Contains specified values to switch dataEndpointPost handling input object converted to {@link ActionFlowData}.\nAvailable values:\n  - AFDM - take given object\n  - ARRAY - take given object and convert data from `[{ [key: string]: any }, ...]` to `{ \"root\": [{ [key: string]: any }, ...] }`\n  - SINGLE - take given object and convert data from `{ [key: string]: any }` to `{ \"root\": [{ [key: string]: any }] }`",
  "enum": [
    "AFDM",
    "ARRAY",
    "SINGLE"
  ],
  "type": "string"
}
```

### `CryptoHashType`

```json
{
  "properties": {
    "content": {
      "type": "string"
    },
    "iv": {
      "type": "string"
    }
  },
  "required": [
    "content",
    "iv"
  ],
  "type": "object"
}
```

