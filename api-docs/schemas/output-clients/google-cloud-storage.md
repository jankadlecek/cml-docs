# Google Cloud Storage Output Client

> **Context for AI:** Writes files to Google Cloud Storage buckets.
>
> **See also:** [Shared output types](_types.md), [Google auth](../auth/auth-types.md)

### `IGCSOutputClient`

```json
{
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "credentials": {
      "$ref": "#/components/schemas/GoogleCredentialsType",
      "description": "DEPRECATED\nUse authId instead.\n\nCredentials used to authorize client.\nIf not present default credentials are used."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding credentials."
    },
    "bucket": {
      "type": "string",
      "description": "Bucket name in Google Cloud Storage.",
      "example": "my-bucket-name"
    },
    "folderPath": {
      "type": "string",
      "description": "Path to folder where file should be saved.",
      "example": "my/path/to/folder"
    },
    "fileName": {
      "type": "string",
      "description": "Contains fileName without extension.",
      "example": "myFileName"
    },
    "fileExtension": {
      "type": "string",
      "description": "Contains fileExtension without \".\" ."
    },
    "addTimestampSuffix": {
      "type": "boolean",
      "description": "When true a current timestamp is added after fileName. Result file name will be: \"myFileName_123456789.json\""
    },
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler",
      "description": "Provides data conversion from JS to specific output format"
    }
  },
  "required": [
    "fileName",
    "fileExtension",
    "handler"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Pick_IGCSOutputClient.Exclude_keyofIGCSOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "authId": {
      "type": "string",
      "description": "ID to auth component holding credentials."
    },
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler",
      "description": "Provides data conversion from JS to specific output format"
    },
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "credentials": {
      "$ref": "#/components/schemas/GoogleCredentialsType",
      "description": "DEPRECATED\nUse authId instead.\n\nCredentials used to authorize client.\nIf not present default credentials are used."
    },
    "bucket": {
      "type": "string",
      "description": "Bucket name in Google Cloud Storage.",
      "example": "my-bucket-name"
    },
    "folderPath": {
      "type": "string",
      "description": "Path to folder where file should be saved.",
      "example": "my/path/to/folder"
    },
    "fileName": {
      "type": "string",
      "description": "Contains fileName without extension.",
      "example": "myFileName"
    },
    "fileExtension": {
      "type": "string",
      "description": "Contains fileExtension without \".\" ."
    },
    "addTimestampSuffix": {
      "type": "boolean",
      "description": "When true a current timestamp is added after fileName. Result file name will be: \"myFileName_123456789.json\""
    }
  },
  "required": [
    "handler",
    "fileName",
    "fileExtension"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_IGCSOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_IGCSOutputClient.Exclude_keyofIGCSOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

