# Facebook Custom Audience Output Client

> **Context for AI:** Manages Facebook Custom Audiences for advertising.
> Can create audiences and upload user data for ad targeting.
>
> **See also:** [Shared output types](_types.md)

### `ICreateCustomAudience`

```json
{
  "properties": {
    "name": {
      "type": "string"
    },
    "subtype": {
      "type": "string"
    },
    "customer_file_source": {
      "type": "string"
    },
    "description": {
      "type": "string"
    }
  },
  "required": [
    "name",
    "subtype",
    "customer_file_source"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IFacebookCustomAudienceOutputClient`

```json
{
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "accountId": {
      "type": "string"
    },
    "audienceId": {
      "$ref": "#/components/schemas/DynamicContextValueType"
    },
    "authId": {
      "type": "string"
    },
    "customAudience": {
      "$ref": "#/components/schemas/ICreateCustomAudience"
    },
    "schema": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "accountId",
    "audienceId",
    "authId",
    "customAudience",
    "schema"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Pick_IFacebookCustomAudienceOutputClient.Exclude_keyofIFacebookCustomAudienceOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "authId": {
      "type": "string"
    },
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "accountId": {
      "type": "string"
    },
    "audienceId": {
      "$ref": "#/components/schemas/DynamicContextValueType"
    },
    "customAudience": {
      "$ref": "#/components/schemas/ICreateCustomAudience"
    },
    "schema": {
      "items": {
        "type": "string"
      },
      "type": "array"
    }
  },
  "required": [
    "authId",
    "accountId",
    "audienceId",
    "customAudience",
    "schema"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_IFacebookCustomAudienceOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_IFacebookCustomAudienceOutputClient.Exclude_keyofIFacebookCustomAudienceOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

