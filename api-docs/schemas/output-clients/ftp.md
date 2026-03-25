# FTP Output Client

> **Context for AI:** Writes files to FTP/SFTP servers.
> Supports dynamic file naming and multiple output formats (JSON, CSV, XML, EDI).
>
> **See also:** [Shared output types](_types.md), [FTP connection](../connections.md)

### `IFtpOutputClient`

```json
{
  "properties": {
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler"
    },
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "connection": {
      "$ref": "#/components/schemas/IFtpConnection",
      "description": "DEPRECATED\nUse authId instead.\n\nDefines connection details."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding connection details.\nIs required if connection object is not provided."
    },
    "file": {
      "type": "string"
    },
    "pathToFile": {
      "type": "string"
    },
    "dynamicNameParams": {
      "items": {
        "$ref": "#/components/schemas/IFtpDynamicNameParam"
      },
      "type": "array",
      "description": "Provides possibility to generate file name using dynamic params.\nHow does it work:\n\nPossible config:\n```\n const file: string = \"fileName_$PREV_START$_$NOW$.json\";\n const dynamicNameParams: Array<IFtpDynamicNameParam> = [{\n     name: \"$PREV_START$\",\n     contextParam: {\n         name: ContextParamsEnum.prevStart,\n     }\n  },{\n     name: \"$NOW$\",\n     contextParam: {\n         name: ContextParamsEnum.now,\n     }\n  }];\n```\nIn case `prevStart = 12345678945` and `now = 987654321` will generate:\n```\n fileName_12345678945_987654321.json\n```"
    }
  },
  "required": [
    "handler",
    "file"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Pick_IFtpOutputClient.Exclude_keyofIFtpOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "connection": {
      "$ref": "#/components/schemas/IFtpConnection",
      "description": "DEPRECATED\nUse authId instead.\n\nDefines connection details."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding connection details.\nIs required if connection object is not provided."
    },
    "file": {
      "type": "string"
    },
    "pathToFile": {
      "type": "string"
    },
    "dynamicNameParams": {
      "items": {
        "$ref": "#/components/schemas/IFtpDynamicNameParam"
      },
      "type": "array",
      "description": "Provides possibility to generate file name using dynamic params.\nHow does it work:\n\nPossible config:\n```\n const file: string = \"fileName_$PREV_START$_$NOW$.json\";\n const dynamicNameParams: Array<IFtpDynamicNameParam> = [{\n     name: \"$PREV_START$\",\n     contextParam: {\n         name: ContextParamsEnum.prevStart,\n     }\n  },{\n     name: \"$NOW$\",\n     contextParam: {\n         name: ContextParamsEnum.now,\n     }\n  }];\n```\nIn case `prevStart = 12345678945` and `now = 987654321` will generate:\n```\n fileName_12345678945_987654321.json\n```"
    },
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler"
    },
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    }
  },
  "required": [
    "file",
    "handler"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_IFtpOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_IFtpOutputClient.Exclude_keyofIFtpOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

