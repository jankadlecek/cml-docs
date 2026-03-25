# Transformation Action

> **Context for AI:** Transformation actions modify data within the pipeline.
> They use transformation clients (Mapper or Flatter) to reshape data
> between input and output stages.
>
> **See also:** [Common actions](common.md), [Transformation clients](../transformation-clients/)

### `ITransformationActionResultApi`

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

### `TransformationClientTypeEnum`

```json
{
  "enum": [
    "FLATTER",
    "MAPPER"
  ],
  "type": "string"
}
```

### `TransformationClientType`

```json
{
  "anyOf": [
    {
      "$ref": "#/components/schemas/IMapperClient"
    },
    {
      "$ref": "#/components/schemas/IFlatterClient"
    }
  ]
}
```

### `ITransformActionConfigApi`

```json
{
  "properties": {
    "order": {
      "type": "number",
      "format": "double"
    },
    "name": {
      "type": "string"
    },
    "type": {
      "$ref": "#/components/schemas/TransformationClientTypeEnum"
    },
    "skipIfEmpty": {
      "type": "boolean",
      "description": "If true, the action will be skipped if the action flow data is effectively empty.\nEmpty data is defined as: null, undefined, {}, [] and [undefined/null].\n\nUse when it cannot be guaranteed that the action flow data object will contain data.\nThis usually happens when Input action does not generate any data."
    },
    "client": {
      "$ref": "#/components/schemas/TransformationClientType"
    }
  },
  "required": [
    "order",
    "name",
    "type",
    "client"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ITransformationActionBody`

```json
{
  "properties": {
    "contextParams": {
      "$ref": "#/components/schemas/IContextParamsApi"
    },
    "configs": {
      "items": {
        "$ref": "#/components/schemas/ITransformActionConfigApi"
      },
      "type": "array"
    },
    "data": {
      "$ref": "#/components/schemas/ActionFlowData"
    }
  },
  "required": [
    "configs",
    "data"
  ],
  "type": "object",
  "additionalProperties": false
}
```

