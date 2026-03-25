# Flatter Transformation Client

> **Context for AI:** The Flatter is a transformation client that flattens nested data structures.
> It takes hierarchical/nested data and produces flat records suitable for tabular output.
>
> **See also:** [Mapper](mapper.md), [Transformation actions](../actions/transformation-action.md)

### `IFlatterConfig`

```json
{
  "properties": {
    "rootOutName": {
      "type": "string"
    },
    "outNameDelimiter": {
      "type": "string",
      "description": "Provides possibility to change default delimiter.\nDefault delimiter is `\".\"`.\n\nExample:\n\nInput object for flattering process:\n```\n{\n    \"person\": {\n        \"name\": \"Test\",\n        \"surname\": \"Tester\",\n        \"address\": {\n            \"street\": \"Test street 1234\",\n            \"city\": \"Testnama\"\n        }\n    }\n}\n```\nResult with default delimiter:\n```\n{\n   \"person.name\": \"Test\",\n   \"person.surname\": \"Tester\",\n   \"person.address.street\": \"Test street 1234\",\n   \"person.address.city\": \"Testnama\",\n}\n```\n\nResult with `outNameDelimiter = \"_\"`:\n```\n{\n   \"person_name\": \"Test\",\n   \"person_surname\": \"Tester\",\n   \"person_address_street\": \"Test street 1234\",\n   \"person_address_city\": \"Testnama\",\n}\n```",
      "example": "_"
    },
    "mappingConfigs": {
      "items": {
        "$ref": "#/components/schemas/MappingConfig"
      },
      "type": "array"
    }
  },
  "required": [
    "rootOutName"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IFlatterMappingsType`

```json
{
  "properties": {},
  "additionalProperties": {
    "$ref": "#/components/schemas/IFlatterConfig"
  },
  "type": "object"
}
```

### `IFlatterClient`

```json
{
  "properties": {
    "mappings": {
      "$ref": "#/components/schemas/IFlatterMappingsType"
    }
  },
  "required": [
    "mappings"
  ],
  "type": "object",
  "additionalProperties": false
}
```

