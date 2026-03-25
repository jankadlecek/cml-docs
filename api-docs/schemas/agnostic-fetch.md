# Agnostic Fetch Schemas

> **Context for AI:** Schemas for the Agnostic Fetch module that handles
> e-commerce platform integrations (currently Shoptet).
>
> **See also:** [Agnostic Fetch endpoints](../endpoints/agnostic-fetch.md)

### `EService`

```json
{
  "enum": [
    "shoptet"
  ],
  "type": "string"
}
```

### `RefreshAgnosticFetchInput`

Refresh input

```json
{
  "description": "Refresh input",
  "properties": {
    "snwfServiceAccount": {
      "$ref": "#/components/schemas/ConnectionOptions"
    },
    "cmlServiceAccountId": {
      "type": "string"
    },
    "cmlProdProjectId": {
      "type": "string"
    },
    "service": {
      "$ref": "#/components/schemas/EService"
    },
    "externalId": {
      "type": "string"
    },
    "authId": {
      "type": "string"
    },
    "recipients": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "cronPattern": {
      "type": "string"
    },
    "productParameters": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "convertToCurrency": {
      "type": "string"
    },
    "currencyConversion": {
      "type": "boolean"
    },
    "fromCurrencies": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "incRefreshOrchestrationId": {
      "type": "string"
    },
    "fullRefreshOrchestrationId": {
      "type": "string"
    },
    "fullRefreshTEOrchestrationId": {
      "type": "string"
    },
    "incRefreshTEOrchestrationId": {
      "type": "string"
    },
    "updateRefreshTEOrchestrationId": {
      "type": "string"
    },
    "portalCompanyId": {
      "type": "number",
      "format": "double"
    },
    "portalProjectId": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "snwfServiceAccount",
    "cmlServiceAccountId",
    "cmlProdProjectId",
    "service",
    "externalId",
    "authId",
    "recipients",
    "cronPattern",
    "productParameters",
    "convertToCurrency",
    "currencyConversion",
    "fromCurrencies",
    "portalCompanyId",
    "portalProjectId"
  ],
  "type": "object",
  "additionalProperties": false
}
```

