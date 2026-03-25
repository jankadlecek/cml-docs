# Shared Types

> **Context for AI:** Utility types shared across multiple domains in CML.
> Includes pagination info, generic data containers, and system statistics.

### `IPageInfo`

Page info.

```json
{
  "description": "Page info.",
  "properties": {
    "offset": {
      "type": "integer",
      "format": "int32"
    },
    "limit": {
      "type": "integer",
      "format": "int32"
    }
  },
  "required": [
    "offset",
    "limit"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `v8.DoesZapCodeSpaceFlag`

```json
{
  "type": "number",
  "enum": [
    0,
    1
  ]
}
```

### `v8.HeapInfo`

```json
{
  "properties": {
    "total_heap_size": {
      "type": "number",
      "format": "double"
    },
    "total_heap_size_executable": {
      "type": "number",
      "format": "double"
    },
    "total_physical_size": {
      "type": "number",
      "format": "double"
    },
    "total_available_size": {
      "type": "number",
      "format": "double"
    },
    "used_heap_size": {
      "type": "number",
      "format": "double"
    },
    "heap_size_limit": {
      "type": "number",
      "format": "double"
    },
    "malloced_memory": {
      "type": "number",
      "format": "double"
    },
    "peak_malloced_memory": {
      "type": "number",
      "format": "double"
    },
    "does_zap_garbage": {
      "$ref": "#/components/schemas/v8.DoesZapCodeSpaceFlag"
    },
    "number_of_native_contexts": {
      "type": "number",
      "format": "double"
    },
    "number_of_detached_contexts": {
      "type": "number",
      "format": "double"
    },
    "total_global_handles_size": {
      "type": "number",
      "format": "double"
    },
    "used_global_handles_size": {
      "type": "number",
      "format": "double"
    },
    "external_memory": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "total_heap_size",
    "total_heap_size_executable",
    "total_physical_size",
    "total_available_size",
    "used_heap_size",
    "heap_size_limit",
    "malloced_memory",
    "peak_malloced_memory",
    "does_zap_garbage",
    "number_of_native_contexts",
    "number_of_detached_contexts",
    "total_global_handles_size",
    "used_global_handles_size",
    "external_memory"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Record_string.any_`

Construct a type with a set of properties K of type T

```json
{
  "properties": {},
  "additionalProperties": {},
  "type": "object",
  "description": "Construct a type with a set of properties K of type T"
}
```

### `ISystemStatistics`

```json
{
  "properties": {
    "memory": {
      "$ref": "#/components/schemas/v8.HeapInfo",
      "description": "Heap info. For more information please refer to v8"
    },
    "memoryUsage": {
      "$ref": "#/components/schemas/Record_string.any_",
      "description": "Current Memory usage for the process."
    }
  },
  "required": [
    "memory",
    "memoryUsage"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Record_string.ActionFlowDataArrayType_`

Construct a type with a set of properties K of type T

```json
{
  "properties": {},
  "additionalProperties": {
    "$ref": "#/components/schemas/ActionFlowDataArrayType"
  },
  "type": "object",
  "description": "Construct a type with a set of properties K of type T"
}
```

### `ActionFlowDataItem`

```json
{
  "properties": {},
  "additionalProperties": {},
  "type": "object"
}
```

### `ActionFlowData`

```json
{
  "properties": {},
  "additionalProperties": {
    "items": {
      "$ref": "#/components/schemas/ActionFlowDataItem"
    },
    "type": "array"
  },
  "type": "object"
}
```

### `Record_string.ActionResultApi_`

Construct a type with a set of properties K of type T

```json
{
  "properties": {},
  "additionalProperties": {
    "$ref": "#/components/schemas/ActionResultApi"
  },
  "type": "object",
  "description": "Construct a type with a set of properties K of type T"
}
```

