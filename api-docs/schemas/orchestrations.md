# Orchestration Schemas

> **Context for AI:** Orchestrations define complete data pipelines in CML.
> An orchestration config specifies actions to run, scheduling, and data endpoints.
> Orchestration runs track execution history with status and results.
>
> **See also:** [Orchestrations endpoints](../endpoints/orchestrations.md), [Actions](actions/)

### `IOrchestrationRunItemApi`

```json
{
  "properties": {
    "id": {
      "type": "string",
      "description": "Resource's unique ID"
    },
    "createdAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s creation date with time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s last update date with time"
    },
    "configurationId": {
      "type": "string"
    },
    "state": {
      "$ref": "#/components/schemas/JobStateEnum"
    }
  },
  "required": [
    "id",
    "createdAt",
    "updatedAt",
    "configurationId",
    "state"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IListOrchestrationRunResponse`

```json
{
  "properties": {
    "count": {
      "type": "integer",
      "format": "int32"
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/IOrchestrationRunItemApi"
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

### `ExtendedLimitEnum`

```json
{
  "enum": [
    100,
    250,
    500,
    1000
  ],
  "type": "number"
}
```

### `IOrchestrationConfigApi`

```json
{
  "properties": {
    "id": {
      "type": "string",
      "description": "Resource's unique ID"
    },
    "createdAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s creation date with time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s last update date with time"
    },
    "name": {
      "type": "string"
    },
    "nextRunTimeStamp": {
      "type": "number",
      "format": "double"
    },
    "lastRunTimeStamp": {
      "type": "number",
      "format": "double"
    },
    "cronPattern": {
      "type": "string",
      "nullable": true
    },
    "actions": {
      "items": {
        "$ref": "#/components/schemas/IActionConfigApi"
      },
      "type": "array"
    },
    "trigger": {
      "$ref": "#/components/schemas/ITriggerActionConfigApi"
    },
    "state": {
      "$ref": "#/components/schemas/JobConfigStateEnum"
    },
    "hasJobInProcess": {
      "type": "boolean"
    },
    "recipients": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "id",
    "createdAt",
    "updatedAt",
    "name",
    "nextRunTimeStamp",
    "cronPattern",
    "actions",
    "state",
    "recipients"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IListOrchestrationConfigResponse`

```json
{
  "properties": {
    "count": {
      "type": "integer",
      "format": "int32"
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/IOrchestrationConfigApi"
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

### `IOrchestrationConfigCreateApi`

```json
{
  "properties": {
    "name": {
      "type": "string"
    },
    "cronPattern": {
      "type": "string",
      "nullable": true
    },
    "actions": {
      "items": {
        "$ref": "#/components/schemas/IActionConfigApi"
      },
      "type": "array"
    },
    "trigger": {
      "$ref": "#/components/schemas/ITriggerActionConfigApi"
    },
    "recipients": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "name",
    "cronPattern",
    "actions",
    "recipients"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IOrchestrationRunApi`

```json
{
  "properties": {
    "id": {
      "type": "string",
      "description": "Resource's unique ID"
    },
    "createdAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s creation date with time"
    },
    "updatedAt": {
      "type": "string",
      "format": "date-time",
      "description": "Resource`s last update date with time"
    },
    "configurationId": {
      "type": "string"
    },
    "state": {
      "$ref": "#/components/schemas/JobStateEnum"
    },
    "actionResult": {
      "items": {
        "$ref": "#/components/schemas/GenericResultType"
      },
      "type": "array"
    }
  },
  "required": [
    "id",
    "createdAt",
    "updatedAt",
    "state"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IOrchestrationConfigUpdateApi`

```json
{
  "properties": {
    "name": {
      "type": "string"
    },
    "cronPattern": {
      "type": "string",
      "nullable": true
    },
    "actions": {
      "items": {
        "$ref": "#/components/schemas/IActionConfigApi"
      },
      "type": "array"
    },
    "trigger": {
      "$ref": "#/components/schemas/ITriggerActionConfigApi"
    },
    "changeNextRun": {
      "type": "boolean"
    },
    "recipients": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "name",
    "cronPattern",
    "actions",
    "changeNextRun",
    "recipients"
  ],
  "type": "object",
  "additionalProperties": false
}
```

