# Common Action Types

> **Context for AI:** Actions are the building blocks of CML orchestrations.
> Each orchestration consists of a sequence of actions that process data:
> 1. **Input Action** → reads data from a source
> 2. **Transformation Action** → transforms the data
> 3. **Output Action** → writes data to a destination
> 4. **Trigger Action** → triggers external processes (e.g., dbt)
>
> Actions can be nested (child actions) and have pre-actions.
> This file contains the common types shared across all action types.
>
> **See also:** [input](input-action.md), [output](output-action.md), [transformation](transformation-action.md), [trigger](trigger-action.md)

### `ActionResultApi`

```json
{
  "properties": {
    "status": {
      "$ref": "#/components/schemas/GenericResultStatusEnum"
    },
    "error": {
      "type": "string"
    }
  },
  "required": [
    "status"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ICommonActionResult`

```json
{
  "properties": {
    "status": {
      "$ref": "#/components/schemas/GenericResultStatusEnum"
    },
    "error": {
      "type": "string"
    },
    "resultHook": {
      "$ref": "#/components/schemas/IResultHookInfo"
    }
  },
  "required": [
    "status"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IActionPartResult`

```json
{
  "properties": {},
  "type": "object",
  "additionalProperties": {
    "$ref": "#/components/schemas/ICommonActionResult"
  }
}
```

### `IGenericActionResult`

```json
{
  "properties": {
    "type": {
      "type": "string"
    },
    "status": {
      "$ref": "#/components/schemas/GenericResultStatusEnum"
    },
    "errorMessage": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "results": {
      "$ref": "#/components/schemas/IActionPartResult"
    }
  },
  "required": [
    "type",
    "status"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IActionStepStatus`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "name": {
      "type": "string"
    },
    "type": {
      "type": "string"
    },
    "status": {
      "anyOf": [
        {
          "$ref": "#/components/schemas/GenericProcessingStatusEnum"
        },
        {
          "$ref": "#/components/schemas/GenericResultStatusEnum"
        }
      ]
    },
    "error": {
      "type": "string",
      "nullable": true
    },
    "started": {
      "type": "number",
      "format": "double"
    },
    "finished": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "id",
    "name",
    "type",
    "status"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IActionStatus`

```json
{
  "properties": {
    "order": {
      "type": "number",
      "format": "double"
    },
    "currentSteps": {
      "items": {
        "$ref": "#/components/schemas/IActionStepStatus"
      },
      "type": "array"
    },
    "completedSteps": {
      "items": {
        "$ref": "#/components/schemas/IActionStepStatus"
      },
      "type": "array"
    },
    "started": {
      "type": "number",
      "format": "double"
    },
    "finished": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "order",
    "currentSteps",
    "completedSteps"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IActionConfigApi`

Contains basic configuration of action to be executed.
Inner action configs are executed in this order:
 - input
 - transformations - optional
 - output
 - children - optional
 - trigger - optional

```json
{
  "description": "Contains basic configuration of action to be executed.\nInner action configs are executed in this order:\n - input\n - transformations - optional\n - output\n - children - optional\n - trigger - optional",
  "properties": {
    "children": {
      "items": {
        "$ref": "#/components/schemas/IChildActionConfigApi"
      },
      "type": "array"
    },
    "debugLog": {
      "type": "boolean"
    },
    "input": {
      "$ref": "#/components/schemas/IInputActionConfigApi"
    },
    "order": {
      "type": "number",
      "format": "double"
    },
    "output": {
      "$ref": "#/components/schemas/OutputActionConfigMappingApi"
    },
    "transformations": {
      "items": {
        "$ref": "#/components/schemas/ITransformActionConfigApi"
      },
      "type": "array"
    },
    "trigger": {
      "$ref": "#/components/schemas/ITriggerActionConfigApi"
    }
  },
  "required": [
    "input",
    "order"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IPreChildActionApi`

Contains all required data to be run.

```json
{
  "description": "Contains all required data to be run.",
  "properties": {
    "config": {
      "$ref": "#/components/schemas/IOutputActionConfig",
      "description": "Only Output action are support for now."
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/ActionFlowData"
      },
      "type": "array",
      "description": "To truncate data use the empty array `[]` for data."
    }
  },
  "required": [
    "config",
    "data"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IChildActionConfigApi`

Provides child configuration.

```json
{
  "description": "Provides child configuration.",
  "properties": {
    "action": {
      "$ref": "#/components/schemas/IActionConfigApi",
      "description": "Actions are run for all parent data as separate action."
    },
    "actionName": {
      "type": "string"
    },
    "parentDataSourceName": {
      "type": "string"
    },
    "path": {
      "type": "string"
    },
    "paramName": {
      "type": "string"
    },
    "paramType": {
      "$ref": "#/components/schemas/DynamicParamTypeEnum"
    },
    "parallelProcessing": {
      "type": "boolean",
      "description": "Optional param. If true parent collection is processed using parallel processing."
    },
    "passParentDataToContext": {
      "type": "boolean",
      "description": "Required param. Switch between two logics of child evaluation.\nIf true parent data are passed to each child and can be used in child actions. Otherwise, parent data is not accessible to use.",
      "example": "true"
    },
    "preChildAction": {
      "items": {
        "$ref": "#/components/schemas/IPreChildActionApi"
      },
      "type": "array",
      "description": "All preChildActions are run only once before all actions."
    }
  },
  "required": [
    "action",
    "actionName",
    "parentDataSourceName",
    "path",
    "paramName",
    "paramType"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IActionBody`

```json
{
  "properties": {
    "contextParams": {
      "$ref": "#/components/schemas/IContextParamsApi"
    },
    "action": {
      "$ref": "#/components/schemas/IActionConfigApi",
      "description": "Contain an action to be run.\nRequired attribute."
    }
  },
  "required": [
    "action"
  ],
  "type": "object",
  "additionalProperties": false
}
```

