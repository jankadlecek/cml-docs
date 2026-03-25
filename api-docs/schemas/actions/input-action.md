# Input Action

> **Context for AI:** Input actions read data from external sources into the CML pipeline.
> They wrap an input client (HTTP, FTP, database, etc.) with action-level configuration
> including context parameters and validation.
>
> **See also:** [Common actions](common.md), [Input clients](../input-clients/)

### `InputActionResultApi`

```json
{
  "properties": {
    "status": {
      "$ref": "#/components/schemas/GenericResultStatusEnum"
    },
    "error": {
      "type": "string"
    },
    "data": {
      "$ref": "#/components/schemas/ActionFlowDataType"
    },
    "size": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "status"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IInputActionConfigApi`

```json
{
  "properties": {
    "resultHookClient": {
      "items": {
        "$ref": "#/components/schemas/IHttpResultHookClientConfig"
      },
      "type": "array",
      "description": "Result hook client\n\nUse to send status via HTTP request to external system after the output client is executed.\nMapper is used to adjust the final object to be sent in request body.\n\nOffers possibility to send data to external system in case of success or error.\n\nSUCCESS object has following structure:\n\n```json\n{\n  \"actionFlowData\": data coming from Input Client or into Output Client,\n  \"clientResponse\": OutputClientResult / null\n}\n```\n\nERROR object has following structure:\n\n```json\n{\n  \"error\": complete error message,\n  \"errorObject\": Error object that can contain additional information about the error,\n  \"name\": name of the faulty client,\n  \"status\": \"ERROR\"\n}\n```"
    },
    "name": {
      "type": "string",
      "description": "Required attribute. Contains name of Input Action.",
      "example": "my super input action name"
    },
    "type": {
      "$ref": "#/components/schemas/InputClientTypeEnum",
      "description": "Required attribute. Defines type of configuration used in *client* attribute"
    },
    "client": {
      "$ref": "#/components/schemas/InputClientConfigType",
      "description": "Contains specific client configuration for data load. For type `DATA_ENDPOINT` use `null`."
    },
    "validationSchema": {
      "$ref": "#/components/schemas/IInputValidationSchema",
      "description": "Schema to validate input data"
    }
  },
  "required": [
    "name",
    "type",
    "client"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IContextParamsApi`

```json
{
  "properties": {
    "now": {
      "type": "number",
      "format": "double",
      "description": "Contains unix time in `ms`. Now is filled when orchestration / job is started.",
      "example": "1659535075000"
    },
    "prevStart": {
      "type": "number",
      "format": "double",
      "description": "Contains unix time in `ms`.",
      "example": "1659525075000"
    },
    "runId": {
      "type": "string",
      "description": "Contains RunId if orchestration starts as run (Job). In other cases (Playground, test) contains Request ActionId by default.",
      "example": "a3125591-3c13-432b-a637-2c32581ee483"
    },
    "parentData": {
      "$ref": "#/components/schemas/ActionFlowDataItem",
      "description": "Optional attribute filled only for child process."
    },
    "projectId": {
      "type": "string",
      "description": "Contains Project id if orchestration starts as run (Job). In other cases (Playground, test) contains Request ActionId by default.",
      "example": "a3125591-3c13-432b-a637-2c32581ee483"
    },
    "orchestrationId": {
      "type": "string",
      "description": "Contains Orchestration id if orchestration starts as run (Job). In other cases (Playground, test) contains Request ActionId by default.",
      "example": "a3125591-3c13-432b-a637-2c32581ee483"
    }
  },
  "required": [
    "now",
    "prevStart",
    "runId",
    "projectId",
    "orchestrationId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IPlaygroundInputActionBody`

```json
{
  "properties": {
    "contextParams": {
      "$ref": "#/components/schemas/IContextParamsApi"
    },
    "config": {
      "$ref": "#/components/schemas/IInputActionConfigApi"
    },
    "data": {
      "$ref": "#/components/schemas/ActionFlowData",
      "description": "This is usable only for DATA_ENDPOINT input type. As all the other input client types fetch data from external source,"
    }
  },
  "required": [
    "config"
  ],
  "type": "object",
  "additionalProperties": false
}
```

