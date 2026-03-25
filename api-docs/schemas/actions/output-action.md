# Output Action

> **Context for AI:** Output actions write processed data to external destinations.
> They wrap an output client with mapping configuration that transforms
> the pipeline data into the format expected by the destination.
>
> **See also:** [Common actions](common.md), [Output clients](../output-clients/)

### `OutputActionResultApi`

```json
{
  "properties": {
    "status": {
      "$ref": "#/components/schemas/GenericResultStatusEnum"
    },
    "error": {
      "type": "string"
    },
    "results": {
      "$ref": "#/components/schemas/Record_string.ActionResultApi_"
    }
  },
  "required": [
    "status"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `OutputActionConfigMappingApi`

```json
{
  "properties": {},
  "additionalProperties": {
    "$ref": "#/components/schemas/IOutputActionConfig"
  },
  "type": "object"
}
```

### `IOutputActionBody`

```json
{
  "properties": {
    "contextParams": {
      "$ref": "#/components/schemas/IContextParamsApi"
    },
    "config": {
      "$ref": "#/components/schemas/OutputActionConfigMappingApi"
    },
    "data": {
      "$ref": "#/components/schemas/ActionFlowData"
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

