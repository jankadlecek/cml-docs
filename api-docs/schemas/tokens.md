# Token Schemas

> **Context for AI:** Tokens provide API access authentication.
> Types include standard API tokens and embed tokens for embedded UI components.
>
> **See also:** [Components endpoints](../endpoints/components.md)

### `ITokenCreateResponse`

Token Create Response

```json
{
  "description": "Token Create Response",
  "properties": {
    "tokenId": {
      "type": "string"
    },
    "secret": {
      "type": "string"
    }
  },
  "required": [
    "tokenId",
    "secret"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ITokenPayload`

Token Payload Data.

```json
{
  "description": "Token Payload Data.",
  "properties": {},
  "type": "object",
  "additionalProperties": {}
}
```

### `ITokenCreate`

Token Create Body

```json
{
  "description": "Token Create Body",
  "properties": {
    "name": {
      "type": "string",
      "description": "Required param.",
      "example": "My Token Name"
    },
    "payload": {
      "$ref": "#/components/schemas/ITokenPayload",
      "description": "Optional param for specific data to be added to token."
    },
    "type": {
      "$ref": "#/components/schemas/TokenTypeEnum"
    }
  },
  "required": [
    "name",
    "type"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ITokenGenerateResponse`

Generated Token

```json
{
  "description": "Generated Token",
  "properties": {
    "token": {
      "type": "string"
    }
  },
  "required": [
    "token"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IGenerateEmbedTokenApiResponse`

Generated Embed Token

```json
{
  "description": "Generated Embed Token",
  "properties": {
    "embedToken": {
      "type": "string"
    },
    "expires": {
      "type": "string"
    }
  },
  "required": [
    "embedToken",
    "expires"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IGenerateEmbedTokenApi`

```json
{
  "properties": {
    "reportId": {
      "type": "string",
      "description": "Contains report ID of the PBI report.",
      "example": "1234-567894684-asdjfhsalkf-sfsf"
    },
    "workspaceId": {
      "type": "string",
      "description": "Contains workspace ID of the PBI report.",
      "example": "1234-567894684-asdjfhsalkf-sfsf"
    }
  },
  "required": [
    "reportId",
    "workspaceId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IGenerateMultiEmbedTokenApi`

```json
{
  "properties": {
    "reportIds": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "description": "List of report IDs of the PBI reports.",
      "example": [
        "1234-567894684-asdjfhsalkf-sfsf"
      ]
    },
    "datasetIds": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "description": "List of dataset IDs used within the PBI reports.",
      "example": [
        "1234-567894684-asdjfhsalkf-sfsf"
      ]
    }
  },
  "required": [
    "reportIds",
    "datasetIds"
  ],
  "type": "object",
  "additionalProperties": false
}
```

