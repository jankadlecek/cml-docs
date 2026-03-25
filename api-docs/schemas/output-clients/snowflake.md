# Snowflake Output Client

> **Context for AI:** Loads data into Snowflake tables.
> Supports multiple write modes, file formats, compression, and column definitions.
>
> **Key concepts:**
> - **WriteMode**: How data is written (append, truncate, merge, etc.)
> - **FileFormat**: Staging file format (CSV, JSON, Parquet)
> - **Compression**: File compression for staging (GZIP, SNAPPY, etc.)
>
> **See also:** [Shared output types](_types.md), [Snowflake connection](../connections.md)

### `SnowflakeFileTypeEnum`

```json
{
  "enum": [
    "CSV",
    "JSON",
    "AVRO",
    "ORC",
    "PARQUET",
    "XML"
  ],
  "type": "string"
}
```

### `SnowflakeFileCompressionEnum`

```json
{
  "enum": [
    "AUTO",
    "GZIP",
    "BZ2",
    "BROTLI",
    "ZSTD",
    "DEFLATE",
    "RAW_DEFLATE",
    "NONE"
  ],
  "type": "string"
}
```

### `ISnowflakeFileFormat`

```json
{
  "properties": {
    "type": {
      "$ref": "#/components/schemas/SnowflakeFileTypeEnum"
    },
    "compression": {
      "$ref": "#/components/schemas/SnowflakeFileCompressionEnum"
    }
  },
  "required": [
    "type"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `SnowflakeColumnTypeEnum`

```json
{
  "enum": [
    "ARRAY",
    "BOOLEAN",
    "STRING",
    "NUMBER",
    "VARIANT",
    "OBJECT"
  ],
  "type": "string"
}
```

### `ISnowflakeColumnDefinition`

Provides colum schema definition used for table creation.

```json
{
  "description": "Provides colum schema definition used for table creation.",
  "properties": {
    "name": {
      "type": "string",
      "description": "Column name. Please refer to [Snowflake Identifier doc](https://docs.snowflake.com/en/sql-reference/identifiers-syntax.html).",
      "example": "my_column_name"
    },
    "type": {
      "anyOf": [
        {
          "$ref": "#/components/schemas/SnowflakeColumnTypeEnum"
        },
        {
          "type": "string"
        }
      ],
      "description": "Column type",
      "example": "string | variant | string(10) | number"
    },
    "customTypeHandler": {
      "type": "string",
      "description": "Provides custom handler function to convert data to desired Snowflake data type. Mostly used to string / date conversion. For more conversion function refer to Snowflake\ndocumentation.",
      "example": "to_timestamp(__COLUMN_NAME__)"
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

### `SnowflakeWriteMode`

```json
{
  "enum": [
    "APPEND",
    "TRUNCATE"
  ],
  "type": "string"
}
```

### `ISnowflakeOutputClient`

```json
{
  "properties": {
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "connection": {
      "$ref": "#/components/schemas/ISnowflakeConnection",
      "description": "DEPRECATED\nUse authId instead.\n\nProvides connection detail to Snowflake."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding connection details.\nIs required if connection object is not provided."
    },
    "adminConnection": {
      "$ref": "#/components/schemas/IAdminSnowflakeConnection",
      "description": "DEPRECATED\nUse adminAuthId instead.\n\nProvides connection detail for admin access to the stage GCS"
    },
    "adminAuthId": {
      "type": "string",
      "description": "ID to auth component holding connection details for ADMIN access to Snowflake."
    },
    "useGCS": {
      "type": "boolean",
      "description": "If `true` the logic of the upload data to the Snowflake is changed for larger files or data using variants.\n   - uses GCS instead of local stage",
      "example": "true"
    },
    "dataDownload": {
      "type": "boolean",
      "description": "If `true` input needs to provide a url from which the data is downloaded"
    },
    "fileFormat": {
      "$ref": "#/components/schemas/ISnowflakeFileFormat",
      "description": "If data is being downloaded from a url, context for file type needs to be provided\nJSON type and no compression is used by default\nFor more information please refer to [Snowflake Stage doc](https://docs.snowflake.com/en/sql-reference/sql/create-stage)"
    },
    "tableSchema": {
      "items": {
        "$ref": "#/components/schemas/ISnowflakeColumnDefinition"
      },
      "type": "array",
      "description": "Optional attribute used to override automatic schema detection.\n\nRules for user schema definition\n- column names must be same as object keys `\"id\" === \"id\" | \"id\" !== \"ID\"`\n- default data conversion is provided by Snowflake, so dates and some special attributes may not be applied correctly in some corner cases\n\nAutomatic schema detection currently supports Snowflake column types:\n- VARIANT - when inner attribute is a JSON object or an array\n- STRING - for other cases (string, number, date, ect.)\n    - to add other data types please contact our support"
    },
    "tableName": {
      "type": "string",
      "description": "Table name where data should be inserted. Please refer to [Snowflake Identifier doc](https://docs.snowflake.com/en/sql-reference/identifiers-syntax.html)",
      "example": "MY_TABLE"
    },
    "writeMode": {
      "$ref": "#/components/schemas/SnowflakeWriteMode",
      "description": "Table name where data should be inserted. Now we support this types:\n- SnowflakeWriteMode#TRUNCATE - if table exists provides table recreation with given schema (automatically determined or user schema definition)\n- {@link SnowflakeWriteMode#APPEND } - if table exists provides appending data to existing table\n    - recommended way is to use mapper / flatter for automatic schema detection or use user schema definition to remove JSON schema diversity.",
      "example": "APPEND"
    }
  },
  "required": [
    "tableName",
    "writeMode"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Pick_ISnowflakeOutputClient.Exclude_keyofISnowflakeOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "connection": {
      "$ref": "#/components/schemas/ISnowflakeConnection",
      "description": "DEPRECATED\nUse authId instead.\n\nProvides connection detail to Snowflake."
    },
    "authId": {
      "type": "string",
      "description": "ID to auth component holding connection details.\nIs required if connection object is not provided."
    },
    "retryable": {
      "$ref": "#/components/schemas/IRetryable"
    },
    "adminConnection": {
      "$ref": "#/components/schemas/IAdminSnowflakeConnection",
      "description": "DEPRECATED\nUse adminAuthId instead.\n\nProvides connection detail for admin access to the stage GCS"
    },
    "adminAuthId": {
      "type": "string",
      "description": "ID to auth component holding connection details for ADMIN access to Snowflake."
    },
    "useGCS": {
      "type": "boolean",
      "description": "If `true` the logic of the upload data to the Snowflake is changed for larger files or data using variants.\n   - uses GCS instead of local stage",
      "example": "true"
    },
    "dataDownload": {
      "type": "boolean",
      "description": "If `true` input needs to provide a url from which the data is downloaded"
    },
    "fileFormat": {
      "$ref": "#/components/schemas/ISnowflakeFileFormat",
      "description": "If data is being downloaded from a url, context for file type needs to be provided\nJSON type and no compression is used by default\nFor more information please refer to [Snowflake Stage doc](https://docs.snowflake.com/en/sql-reference/sql/create-stage)"
    },
    "tableSchema": {
      "items": {
        "$ref": "#/components/schemas/ISnowflakeColumnDefinition"
      },
      "type": "array",
      "description": "Optional attribute used to override automatic schema detection.\n\nRules for user schema definition\n- column names must be same as object keys `\"id\" === \"id\" | \"id\" !== \"ID\"`\n- default data conversion is provided by Snowflake, so dates and some special attributes may not be applied correctly in some corner cases\n\nAutomatic schema detection currently supports Snowflake column types:\n- VARIANT - when inner attribute is a JSON object or an array\n- STRING - for other cases (string, number, date, ect.)\n    - to add other data types please contact our support"
    },
    "tableName": {
      "type": "string",
      "description": "Table name where data should be inserted. Please refer to [Snowflake Identifier doc](https://docs.snowflake.com/en/sql-reference/identifiers-syntax.html)",
      "example": "MY_TABLE"
    },
    "writeMode": {
      "$ref": "#/components/schemas/SnowflakeWriteMode",
      "description": "Table name where data should be inserted. Now we support this types:\n- SnowflakeWriteMode#TRUNCATE - if table exists provides table recreation with given schema (automatically determined or user schema definition)\n- {@link SnowflakeWriteMode#APPEND } - if table exists provides appending data to existing table\n    - recommended way is to use mapper / flatter for automatic schema detection or use user schema definition to remove JSON schema diversity.",
      "example": "APPEND"
    }
  },
  "required": [
    "tableName",
    "writeMode"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_ISnowflakeOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_ISnowflakeOutputClient.Exclude_keyofISnowflakeOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

