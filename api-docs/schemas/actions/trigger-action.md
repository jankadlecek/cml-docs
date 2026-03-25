# Trigger Action

> **Context for AI:** Trigger actions invoke external processing engines as part of an orchestration.
> Supported triggers:
> - **TransformationEngine**: Run a CML transformation engine
> - **Dbt**: Run dbt models
> - **DataForm**: Run Google DataForm workflows
> - **Keboola**: Trigger Keboola jobs
>
> **See also:** [Common actions](common.md), [Transformation engines](../transformation-engines.md)

### `TriggerClientTypeEnum`

```json
{
  "enum": [
    "KBC",
    "DFR",
    "DBT",
    "TRANSFORMATION_ENGINE"
  ],
  "type": "string"
}
```

### `TERunCommand`

```json
{
  "properties": {
    "order": {
      "type": "number",
      "format": "double",
      "description": "Format: int32"
    },
    "command": {
      "type": "string"
    }
  },
  "required": [
    "order",
    "command"
  ],
  "type": "object"
}
```

### `TERunCommands`

```json
{
  "items": {
    "$ref": "#/components/schemas/TERunCommand"
  },
  "type": "array"
}
```

### `ITransformationEngineTriggerClientConfigApi`

```json
{
  "properties": {
    "maxRetry": {
      "type": "number",
      "format": "double",
      "description": "Max retry number is when result of given config ends with an error."
    },
    "waitForFinalState": {
      "type": "boolean",
      "description": "If true - given trigger waits for final state => OK | ERROR is set after trigger run ends.\nIf false or not present - jut triggers given configuration and not wait for final state. => OK is set as trigger state\n\nCommon final states: `ERROR`, `SUCCESS`, `WARNING`."
    },
    "profileId": {
      "type": "string",
      "description": "Expects Transformation Engine's Profile Id to be used for given run.",
      "example": "some-uu-id-v4"
    },
    "commands": {
      "$ref": "#/components/schemas/TERunCommands",
      "description": "Contains all commands to be executed for given TE Configuration.\nExample:\n```\n[{\n   \"command\": \"run\",\n   \"order\": 0,\n}]\n```"
    },
    "runTimeout": {
      "type": "number",
      "format": "double",
      "description": "Provides possibility to limit / extend TE job timeout. Unit `ms`;",
      "example": "10000"
    },
    "transformationEngineId": {
      "type": "string",
      "description": "Expects Transformation Engine Id to be used for given run.",
      "example": "some-uu-id-v4"
    }
  },
  "required": [
    "profileId",
    "commands",
    "transformationEngineId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IDbtTriggerClientConfigApi`

```json
{
  "properties": {
    "name": {
      "type": "string"
    },
    "accountId": {
      "type": "number",
      "format": "double"
    },
    "jobId": {
      "type": "number",
      "format": "double"
    },
    "maxRetry": {
      "type": "number",
      "format": "double"
    },
    "waitForFinalState": {
      "type": "boolean"
    }
  },
  "required": [
    "name",
    "accountId",
    "jobId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IDataFormTriggerClientConfigApi`

```json
{
  "properties": {
    "name": {
      "type": "string"
    },
    "baseUrl": {
      "type": "string"
    },
    "headers": {
      "properties": {},
      "additionalProperties": {},
      "type": "object"
    },
    "body": {
      "properties": {},
      "additionalProperties": {},
      "type": "object"
    },
    "maxRetry": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "name",
    "baseUrl",
    "headers",
    "body"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IKeboolaTriggerClientConfigApi`

```json
{
  "properties": {
    "maxRetry": {
      "type": "number",
      "format": "double",
      "description": "Max retry number is when result of given config ends with an error."
    },
    "waitForFinalState": {
      "type": "boolean",
      "description": "If true - given trigger waits for final state => OK | ERROR is set after trigger run ends.\nIf false or not present - jut triggers given configuration and not wait for final state. => OK is set as trigger state\n\nCommon final states: `ERROR`, `SUCCESS`, `WARNING`."
    },
    "name": {
      "type": "string",
      "description": "Expects name of the Trigger.",
      "example": "some-name-of-keboola-trigger"
    },
    "method": {
      "type": "string",
      "description": "Expects http method to be used",
      "example": "POST"
    },
    "url": {
      "type": "string",
      "description": "Expects url of the Keboola api. Be sure to use specific location of the Kebbola project.",
      "example": "https://queue.eu-central-1.keboola.com/jobs | https://queue.north-europe.azure.keboola.com/"
    },
    "credentials": {
      "type": "string",
      "description": "Expects token to the Keboola. This will be replaced using auth component in the future."
    },
    "data": {
      "type": "string",
      "description": "Optional param. Data to be sent using the trigger. Expects string value. So if json need to be sent please stringify json object."
    }
  },
  "required": [
    "name",
    "method",
    "url",
    "credentials"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ITriggerClientConfigType`

```json
{
  "anyOf": [
    {
      "$ref": "#/components/schemas/ITransformationEngineTriggerClientConfigApi"
    },
    {
      "$ref": "#/components/schemas/IDbtTriggerClientConfigApi"
    },
    {
      "$ref": "#/components/schemas/IDataFormTriggerClientConfigApi"
    },
    {
      "$ref": "#/components/schemas/IKeboolaTriggerClientConfigApi"
    }
  ]
}
```

### `ITriggerActionConfigApi`

```json
{
  "properties": {
    "forceRun": {
      "type": "boolean"
    },
    "type": {
      "$ref": "#/components/schemas/TriggerClientTypeEnum"
    },
    "client": {
      "$ref": "#/components/schemas/ITriggerClientConfigType"
    }
  },
  "required": [
    "type",
    "client"
  ],
  "type": "object",
  "additionalProperties": false
}
```

