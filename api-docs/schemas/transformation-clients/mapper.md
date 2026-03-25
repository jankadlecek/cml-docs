# Mapper Transformation Client

> **Context for AI:** The Mapper is the primary transformation client in CML.
> It transforms data between input and output by defining field mappings.
>
> **Key concepts:**
> - **MappingItem**: Maps a source field to a destination field with optional translation
> - **TranslationType**: Value transformation (replace, regex, lookup table, etc.)
> - **MappingDefaults**: Default values for unmapped fields
> - **MappingConfig**: Union type for mapper or flatter configuration
>
> **See also:** [Flatter](flatter.md), [Transformation actions](../actions/transformation-action.md)

### `IMapperTranslationType`

```json
{
  "properties": {},
  "additionalProperties": {},
  "type": "object"
}
```

### `IMappingDefaults`

DEPRECATED

```json
{
  "description": "DEPRECATED",
  "properties": {
    "destination": {
      "type": "string"
    },
    "value": {}
  },
  "required": [
    "destination",
    "value"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IMappingItem`

```json
{
  "properties": {
    "contextValue": {
      "$ref": "#/components/schemas/ContextParam",
      "description": "Provides possibility set value from context params."
    },
    "destination": {
      "type": "string",
      "description": "Defines where data will be placed in the result object."
    },
    "transformation": {
      "type": "string",
      "description": "Provides possibility to transform data. Expects ES6 function."
    },
    "translation": {
      "$ref": "#/components/schemas/IMapperTranslationType",
      "description": "Provides possible values for translation. If translation not found input value is used."
    },
    "default": {
      "description": "Defines default value in case input value is null || undefined"
    },
    "value": {
      "description": "Override input value to this value."
    },
    "defaults": {
      "items": {
        "$ref": "#/components/schemas/IMappingDefaults"
      },
      "type": "array",
      "description": "Provides possibility to create additional values in given object context. This is handy if we iterate inner array e.g. \"arr\" `{someData: {..., arr: [{}, {}]}}`, and we\nneed to create default values.\n\nDEPRECATED instead ues multiple object {@link IMappingItem} for given source mapping"
    },
    "createNullValue": {
      "type": "boolean",
      "description": "True => attribute is set to null if original value is null\nFalse || not present => attribute is not created to result object"
    }
  },
  "required": [
    "destination"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IMapperItemsMappingsType`

```json
{
  "properties": {},
  "additionalProperties": {
    "items": {
      "$ref": "#/components/schemas/IMappingItem"
    },
    "type": "array"
  },
  "type": "object"
}
```

### `IMapperConfig`

```json
{
  "properties": {
    "delimiter": {
      "type": "string"
    },
    "mappings": {
      "$ref": "#/components/schemas/IMapperItemsMappingsType"
    }
  },
  "required": [
    "delimiter",
    "mappings"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IMapperMappingsType`

```json
{
  "properties": {},
  "additionalProperties": {
    "$ref": "#/components/schemas/IMapperConfig"
  },
  "type": "object"
}
```

### `IMapperClient`

```json
{
  "properties": {
    "mappings": {
      "$ref": "#/components/schemas/IMapperMappingsType"
    }
  },
  "required": [
    "mappings"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `MappingConfig`

```json
{
  "properties": {
    "skipFlattering": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "description": "Enables to skip flattering process for given attribute name (and all inner object for given attribute)."
    },
    "selectAttributes": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "omitAttributes": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "parentFK": {
      "type": "string"
    },
    "outName": {
      "type": "string"
    },
    "mappingType": {
      "$ref": "#/components/schemas/MappingTypeEnum"
    },
    "attributePath": {
      "type": "string"
    }
  },
  "required": [
    "attributePath"
  ],
  "type": "object"
}
```

