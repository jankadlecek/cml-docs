# Playground Mock Data Schemas

> **Context for AI:** Mock data schemas used by the Playground testing endpoints.
> Provides sample order data for testing input/output actions.
>
> **See also:** [Playground endpoints](../endpoints/playground.md)

### `MockDataOrderListItem`

```json
{
  "properties": {
    "_id": {
      "type": "string"
    },
    "description": {
      "type": "string"
    }
  },
  "required": [
    "_id",
    "description"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IListMockDataOrderPage`

```json
{
  "properties": {
    "count": {
      "type": "integer",
      "format": "int32"
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/MockDataOrderListItem"
      },
      "type": "array",
      "description": "Loaded date for given type"
    },
    "pageInfo": {
      "$ref": "#/components/schemas/IPageInfo",
      "description": "Current page info."
    }
  },
  "required": [
    "count",
    "data",
    "pageInfo"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `MockDataAddress`

```json
{
  "properties": {
    "street": {
      "type": "string"
    },
    "postcode": {
      "type": "string"
    },
    "city": {
      "type": "string"
    },
    "country": {
      "type": "string"
    }
  },
  "required": [
    "street",
    "postcode",
    "city",
    "country"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `MockDataOrderItem`

```json
{
  "properties": {
    "id": {
      "type": "number",
      "format": "double"
    },
    "description": {
      "type": "string"
    },
    "price": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "id",
    "description",
    "price"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `MockDataOrder`

```json
{
  "properties": {
    "_id": {
      "type": "string"
    },
    "index": {
      "type": "string"
    },
    "status": {
      "type": "string"
    },
    "price": {
      "type": "number",
      "format": "double"
    },
    "name": {
      "type": "string"
    },
    "company": {
      "type": "string"
    },
    "email": {
      "type": "string"
    },
    "phone": {
      "type": "string"
    },
    "address": {
      "$ref": "#/components/schemas/MockDataAddress"
    },
    "description": {
      "type": "string"
    },
    "items": {
      "items": {
        "$ref": "#/components/schemas/MockDataOrderItem"
      },
      "type": "array"
    }
  },
  "required": [
    "_id",
    "index",
    "status",
    "price",
    "name",
    "company",
    "email",
    "phone",
    "address",
    "description",
    "items"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IMockToken`

```json
{
  "properties": {
    "secret": {
      "type": "string",
      "description": "Secret used to generate token for Testing",
      "example": "my-secret"
    },
    "payload": {
      "$ref": "#/components/schemas/ITokenPayload",
      "description": "Optional param for specific data to be added to token."
    }
  },
  "required": [
    "secret"
  ],
  "type": "object",
  "additionalProperties": false
}
```

