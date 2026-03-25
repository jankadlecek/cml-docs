# FTP Input Client

> **Context for AI:** The FTP input client reads files from FTP/SFTP servers.
> Used for batch file processing scenarios.
>
> **See also:** [Shared input types](_types.md), [FTP connection](../connections.md)

### `IFtpInputClient`

```json
{
  "properties": {
    "handler": {
      "$ref": "#/components/schemas/IResponseDataHandler"
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
      "type": "string",
      "description": "Filename in source system with extension.",
      "example": "my-file.json"
    },
    "pathToFile": {
      "type": "string",
      "description": "Contains path to file in source system.",
      "example": "/temp/ftp/data"
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

