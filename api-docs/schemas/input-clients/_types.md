# Input Client Shared Types

> **Context for AI:** Shared types used across all input clients. This includes:
> - **Pagination configs**: How to handle paginated API responses (link-based, offset, page number)
> - **HTTP params**: Path params, query params, headers for HTTP requests
> - **Context params**: Dynamic parameters resolved at runtime from parent actions or environment
> - **Response handlers**: How to parse responses (JSON, CSV, XML)
> - **Validation schemas**: Input data validation rules
>
> **See also:** Individual input client files in this folder

### `InputClientTypeEnum`

```json
{
  "enum": [
    "HTTP",
    "G_BIG_QUERY",
    "FTP",
    "DATA_ENDPOINT",
    "SNOWFLAKE",
    "MSSQL",
    "CLICKHOUSE"
  ],
  "type": "string"
}
```

### `HttpPathParams`

All params used in url path.

Result: {{cml_url}}/api/rest/v1/playground/mock/orders/12345

Example:
```
{
 "orderId": "12345"
}

```json
{
  "description": "All params used in url path.\n\nResult: {{cml_url}}/api/rest/v1/playground/mock/orders/12345\n\nExample:\n```\n{\n \"orderId\": \"12345\"\n}",
  "properties": {},
  "type": "object",
  "additionalProperties": {}
}
```

### `HttpQueryParams`

All query params to use in url. Attributes are added automatically to url with prefix ?.

Result: {{cml_url}}/api/rest/v1/playground/mock/orders?from=2021-12-01&to=2022-01-31

Example:
```
{
 "from": "2021-12-01",
 "to": "2022-01-31"
}
```

```json
{
  "description": "All query params to use in url. Attributes are added automatically to url with prefix ?.\n\nResult: {{cml_url}}/api/rest/v1/playground/mock/orders?from=2021-12-01&to=2022-01-31\n\nExample:\n```\n{\n \"from\": \"2021-12-01\",\n \"to\": \"2022-01-31\"\n}\n```",
  "properties": {},
  "type": "object",
  "additionalProperties": {}
}
```

### `HttpHeaders`

All request headers.

Example:
```
{
 "Authorization": "Basic <user-credentials-base64-string>"
}

```json
{
  "description": "All request headers.\n\nExample:\n```\n{\n \"Authorization\": \"Basic <user-credentials-base64-string>\"\n}",
  "properties": {},
  "type": "object",
  "additionalProperties": {}
}
```

### `PaginationMethodEnum`

```json
{
  "enum": [
    "LINK",
    "OFFSET",
    "PAGE"
  ],
  "type": "string"
}
```

### `LinkNextParamDestinationEnum`

```json
{
  "enum": [
    "QUERY",
    "PATH",
    "BODY"
  ],
  "type": "string"
}
```

### `LinkPaginationType`

Defines the link pagination.
How Link pagination works?
- pagination uses link to the next page.
- stop strategies:
   - is next page param's value in the response filled?
      - yes => continue pagination (next condition)
      - no => **STOP**
   - is item count in the response same as `pageLimit`
      - yes => continue pagination (next condition)
      - no => **STOP**
   - is data attribute an array?
      - yes => continue
      - no => **STOP**

```json
{
  "description": "Defines the link pagination.\nHow Link pagination works?\n- pagination uses link to the next page.\n- stop strategies:\n   - is next page param's value in the response filled?\n      - yes => continue pagination (next condition)\n      - no => **STOP**\n   - is item count in the response same as `pageLimit`\n      - yes => continue pagination (next condition)\n      - no => **STOP**\n   - is data attribute an array?\n      - yes => continue\n      - no => **STOP**",
  "properties": {
    "nextParamName": {
      "type": "string",
      "description": "Name of next param. This name is defined by source API.",
      "example": "after | next"
    },
    "nextParamPath": {
      "type": "string",
      "description": "Path to next param in response. This path is defined by source API.\n\n```\nResponse:\n{\n    \"data\": [...someData],\n    \"_links\": {\n      \"after\": \"after_link\",\n      \"before\": \"before_link\",\n    },\n    ...\n}\n```\n\nFrom version `1.3.27` pagination supports possibility to get data from an array.\n- How does it work: For an array (e.g. `data`) pagination takes last item in the array and the attribute defined in the `nextParamPath`.\n- How to use it: just define the path as usual - e.g. `data.id` (where `data` is an array and the `id` is desired attribute to be used as next param)",
      "example": "_links.after"
    },
    "continueOnNonLimitResponse": {
      "type": "boolean",
      "description": "Contains information to override default pagination behavior.\nIf true => when next param is returned to the response and the data on current page did not match limit => continue to next page\nIf false or not present => default behaviour is used = *STOP pagination*",
      "example": "true"
    },
    "startPage": {
      "type": "string",
      "description": "If used, the pagination starts at the given page for provided `pageToken`.",
      "example": "start page token"
    },
    "pageLimit": {
      "type": "integer",
      "format": "int32",
      "description": "Provides result limit per one api call. Mostly defined by source API. Most common values: 10, 25, 50, 100.",
      "example": "100",
      "minimum": 1
    },
    "pageLimitParam": {
      "type": "string",
      "description": "Name of limit param. This name is defined by source API.",
      "example": "limit"
    },
    "nextParamDestination": {
      "$ref": "#/components/schemas/LinkNextParamDestinationEnum",
      "description": "Provides the possibility to define where the next param should be used. Common usage is using query params so this attribute should be set to {@link LinkNextParamDestinationEnum#QUERY }.\n\nFor some specific use cases, link pagination is implemented using full or partial URL replacement. So you need to set this param to {@link * LinkNextParamDestinationEnum#PATH}. To achieve correct behavior you must define URL in {@link IHttpInputClient#url } using dynamic path param. Then the name of this\ndynamic param needs to be filled in {@link LinkPaginationType#nextParamName }.\n\nFor GraphQL APIs or POST requests with JSON body, you can set this to {@link LinkNextParamDestinationEnum#BODY }. In this case, {@link LinkPaginationType#nextParamName }\nshould be a dot-notation path into the body object (e.g., \"variables.cursor\"). The pagination token will be set at that path in the body before each request.\n\nOptional param. Default value is {@link LinkNextParamDestinationEnum#QUERY }"
    },
    "removeQueryParams": {
      "items": {
        "type": "string"
      },
      "type": "array",
      "description": "Possibility to remove specific query params not to be used for pagination in the second call. This removes all given query parameters using names. This is extremely handy in\ncases where pagination returns whole or partial URL already with query params.\nRemoving logic is applied in case the next page param exists in response and is applied before pagination QUERY / PATH attributes are appended to the request."
    }
  },
  "required": [
    "nextParamName",
    "nextParamPath"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `OffsetPaginationType`

Defines an offset pagination.

```json
{
  "description": "Defines an offset pagination.",
  "properties": {
    "offsetParam": {
      "type": "string",
      "description": "Name of offset param. This name is defined by source API.",
      "example": "offset"
    },
    "offset": {
      "type": "integer",
      "format": "int32",
      "description": "If used, the pagination starts at the given offset (e.g. 1000).\n\n0 is used as the default value.",
      "example": "1000"
    },
    "limit": {
      "type": "integer",
      "format": "int32",
      "description": "Provides result limit per one api call. Mostly defined by source API. Most common values: 10, 25, 50, 100.",
      "example": "100",
      "minimum": 1
    },
    "limitParam": {
      "type": "string",
      "description": "Name of limit param. This name is defined by source API.",
      "example": "limit"
    }
  },
  "required": [
    "offsetParam",
    "limit",
    "limitParam"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `PagePaginationType`

Defines the page pagination.

```json
{
  "description": "Defines the page pagination.",
  "properties": {
    "pageParam": {
      "type": "string",
      "description": "Name of page param. This name is defined by source API.",
      "example": "page"
    },
    "firstPage": {
      "type": "integer",
      "format": "int32",
      "description": "If used, the pagination starts at the given page (e.g. 2).\n\n0 is used as the default value.",
      "example": "1"
    },
    "limit": {
      "type": "integer",
      "format": "int32",
      "description": "Provides result limit per one api call. Mostly defined by source API. Most common values: 10, 25, 50, 100.",
      "example": "100",
      "minimum": 1
    },
    "limitParam": {
      "type": "string",
      "description": "Name of limit param. This name is defined by source API.",
      "example": "limit"
    }
  },
  "required": [
    "pageParam",
    "limit",
    "limitParam"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `PaginationConfigType`

```json
{
  "properties": {
    "stopOnErrorCodes": {
      "items": {
        "type": "number",
        "format": "double"
      },
      "type": "array"
    },
    "stopLimit": {
      "type": "number",
      "format": "double"
    },
    "config": {
      "anyOf": [
        {
          "$ref": "#/components/schemas/LinkPaginationType"
        },
        {
          "$ref": "#/components/schemas/OffsetPaginationType"
        },
        {
          "$ref": "#/components/schemas/PagePaginationType"
        }
      ]
    },
    "method": {
      "$ref": "#/components/schemas/PaginationMethodEnum"
    }
  },
  "required": [
    "config",
    "method"
  ],
  "type": "object"
}
```

### `ParentContextParam`

Used to navigate through application context for ContextParamsEnum#parent.

application context example:
```
context: {
    "runId": "some-uu-id",
    "now": 123456789,
    "parentData": {
        "id": "some_parent_id",
        "createdAt": "01.01.1970",
        "tag": "blue",
        "link": {
            "id": "the-link-id",
            "address": {
                "addressId": "address-id",
                ...
            }
            ...
        }
        ....
    },
    ...
}
```

```json
{
  "description": "Used to navigate through application context for ContextParamsEnum#parent.\n\napplication context example:\n```\ncontext: {\n    \"runId\": \"some-uu-id\",\n    \"now\": 123456789,\n    \"parentData\": {\n        \"id\": \"some_parent_id\",\n        \"createdAt\": \"01.01.1970\",\n        \"tag\": \"blue\",\n        \"link\": {\n            \"id\": \"the-link-id\",\n            \"address\": {\n                \"addressId\": \"address-id\",\n                ...\n            }\n            ...\n        }\n        ....\n    },\n    ...\n}\n```",
  "properties": {
    "path": {
      "type": "string",
      "description": "Defines path to get values data from parent. Getting data from arrays is not supported yet.\nAs a starting point is used object `parentData`. Path should start from this point.\n\n```\npath: \"id\" => take value `some_parent_id`\npath: \"link.id\" => take value `the-link-id`\npath: \"link.address.addressId\" => take value `address-id`\npath: \"address\" => throws an error => invalid path to parent\n```",
      "example": "id | link.id | link.address.addressId"
    },
    "delimiter": {
      "type": "string",
      "description": "Optional param. Default value for delimiter is `.`.",
      "example": "_"
    }
  },
  "required": [
    "path"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ContextParam`

```json
{
  "properties": {
    "parentParam": {
      "$ref": "#/components/schemas/ParentContextParam",
      "description": "Optional attribute. Should be used only for child actions (otherwise - an exception is thrown for missing context data). Add possibility to take data from parent and use\nthem in child action."
    },
    "functionArguments": {
      "items": {
        "type": "number",
        "format": "double"
      },
      "type": "array"
    },
    "format": {
      "type": "string",
      "description": "User's specific format to represent date.\n\nThere is multiple possibilities to format date. For simple format please refer to [Moment - Formats](https://momentjs.com/docs/#/parsing/string-format/).\nWe provide additional formats for input date `2022-07-19 16:42:37.986`:\n - `UNIX_TIMESTAMP_DAY_START` = converts date to: `1658188800000 ms`\n - `UNIX_TIMESTAMP_DAY_END` = converts date to: `1658275199999 ms`\n - `UNIX_TIMESTAMP` = converts date to: `1658241757986 ms`\n - `UNIX_TIMESTAMP_SEC_DAY_START` = converts date to: `1658188800 s`\n - `UNIX_TIMESTAMP_SEC_DAY_END` = converts date to: `1658275199 s`\n - `UNIX_TIMESTAMP_SEC` = converts date to: `1658241757 s`",
      "example": "YYYY-MM-DDTHH:mm:ss.SSSZ"
    },
    "type": {
      "$ref": "#/components/schemas/ContextParamTypeEnum",
      "description": "It is possible to chose from two different date formatting:\n - DATE = provides only date formatting\n - CALCULATE_DATE = provides date calculation and date formatting"
    },
    "name": {
      "$ref": "#/components/schemas/ContextParamsEnum"
    }
  },
  "required": [
    "name"
  ],
  "type": "object"
}
```

### `DynamicParamType`

```json
{
  "properties": {
    "transformation": {
      "type": "string",
      "description": "Optional ES6 function string that transforms the resolved value before it is used in the request.\nThe value passed into the function is taken from `value`, `contextParam`, or `authId` (in that\norder). The function receives that value as the single argument and must return the value to use."
    },
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam"
    },
    "type": {
      "$ref": "#/components/schemas/DynamicParamTypeEnum"
    },
    "authId": {
      "type": "string"
    },
    "value": {
      "type": "string"
    },
    "name": {
      "type": "string"
    }
  },
  "required": [
    "type",
    "name"
  ],
  "type": "object"
}
```

### `ResponseHandlerType`

```json
{
  "enum": [
    "CSV",
    "JSON",
    "XML",
    "EDI"
  ],
  "type": "string"
}
```

### `ICsvInputHandler`

Provides CSV format conversion to objects. For more information please refer to [FastCSV](https://www.c2fo.io/fast-csv/docs/parsing/options).

```json
{
  "description": "Provides CSV format conversion to objects. For more information please refer to [FastCSV](https://www.c2fo.io/fast-csv/docs/parsing/options).",
  "properties": {
    "encoding": {
      "type": "string",
      "description": "Encoding to by used for inputHandler / outputHandler communication.\nUTF-8 is used by default.",
      "example": "windows-1250"
    },
    "objectMode": {
      "type": "boolean"
    },
    "delimiter": {
      "type": "string"
    },
    "quote": {
      "type": "string",
      "nullable": true
    },
    "escape": {
      "type": "string"
    },
    "headers": {
      "anyOf": [
        {
          "type": "boolean"
        },
        {
          "items": {
            "type": "string"
          },
          "type": "array"
        }
      ]
    },
    "renameHeaders": {
      "type": "boolean"
    },
    "ignoreEmpty": {
      "type": "boolean"
    },
    "comment": {
      "type": "string"
    },
    "strictColumnHandling": {
      "type": "boolean"
    },
    "discardUnmappedColumns": {
      "type": "boolean"
    },
    "trim": {
      "type": "boolean"
    },
    "ltrim": {
      "type": "boolean"
    },
    "rtrim": {
      "type": "boolean"
    },
    "maxRows": {
      "type": "number",
      "format": "double"
    },
    "skipLines": {
      "type": "number",
      "format": "double"
    },
    "skipRows": {
      "type": "number",
      "format": "double"
    }
  },
  "type": "object",
  "additionalProperties": false
}
```

### `IJsonInputHandler`

Handler attributes to convert data from Json format to JS object(s).

```json
{
  "description": "Handler attributes to convert data from Json format to JS object(s).",
  "properties": {
    "encoding": {
      "type": "string",
      "description": "Encoding to by used for inputHandler / outputHandler communication.\nUTF-8 is used by default.",
      "example": "windows-1250"
    }
  },
  "type": "object",
  "additionalProperties": false
}
```

### `IXmlInputHandler`

Handler attributes to convert from XML format to JS object(s).
For XML conversion we use [xml2js package](https://www.npmjs.com/package/xml2js).

```json
{
  "description": "Handler attributes to convert from XML format to JS object(s).\nFor XML conversion we use [xml2js package](https://www.npmjs.com/package/xml2js).",
  "properties": {
    "encoding": {
      "type": "string",
      "description": "Encoding to by used for inputHandler / outputHandler communication.\nUTF-8 is used by default.",
      "example": "windows-1250"
    }
  },
  "type": "object",
  "additionalProperties": false
}
```

### `IResponseDataHandler`

```json
{
  "properties": {
    "type": {
      "$ref": "#/components/schemas/ResponseHandlerType",
      "description": "Defines type of response data handler configuration used in *config* attribute",
      "example": "JSON || XML"
    },
    "config": {
      "anyOf": [
        {
          "$ref": "#/components/schemas/ICsvInputHandler"
        },
        {
          "$ref": "#/components/schemas/IJsonInputHandler"
        },
        {
          "$ref": "#/components/schemas/IXmlInputHandler"
        }
      ],
      "description": "Use configuration for the given type."
    }
  },
  "required": [
    "type"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IRetryable`

```json
{
  "properties": {
    "maxRetries": {
      "type": "integer",
      "format": "int32",
      "description": "Defines number of retries to be executed. Total number of trys will be 1 + numberOfRetries\n\nRequired param.",
      "example": "3",
      "minimum": 1,
      "maximum": 10
    },
    "delayInSec": {
      "type": "number",
      "format": "double",
      "description": "Specify exact delay between requests in `s`.\nOptional param. If not defined default algorythm is used.\n\nDefault algorithm: <img src=\"https://latex.codecogs.com/svg.latex?\\Large&space;t=b^{(c-2)}\" />, where `b (base) = 2`, `c = current retry index eg. 1, 2, 3,..., 10`.\n\n| No. Retry   | Formula |  Time (s) |\n|----------|:-------------:|------:|\n| 1 |  2^(-1) | 0.5 |\n| 2 |  2^(0) | 1 |\n| 3 | 2^(1) |  2 |\n| 4 |  2^(2) | 4 |\n| 5 |  2^(3) | 8 |\n| 6 | 2^(4) |  16 |\n| 7 |  2^(5) | 32 |\n| 8 |  2^(6) | 64 |\n| 9 | 2^(7) |  128 |\n| 10 | 2^(8) |  256 |"
    }
  },
  "required": [
    "maxRetries"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `InputClientConfigType`

```json
{
  "anyOf": [
    {
      "$ref": "#/components/schemas/IHttpInputClient"
    },
    {
      "$ref": "#/components/schemas/IFtpInputClient"
    },
    {
      "$ref": "#/components/schemas/IQueryGBQInputClient"
    },
    {
      "$ref": "#/components/schemas/ISnowflakeInputClient"
    },
    {
      "$ref": "#/components/schemas/IMssqlInputClient"
    },
    {
      "$ref": "#/components/schemas/IClickHouseInputClient"
    }
  ],
  "nullable": true
}
```

### `IInputValidationSchema`

Schema to validate input data, only applicable to data-endpoint and Snowflake input. More info on the object structure {@link https://ajv.js.org/json-schema.html}

```json
{
  "description": "Schema to validate input data, only applicable to data-endpoint and Snowflake input. More info on the object structure {@link https://ajv.js.org/json-schema.html}",
  "properties": {
    "type": {
      "$ref": "#/components/schemas/ValidationSchemaObjectTypeEnum"
    },
    "properties": {
      "properties": {},
      "additionalProperties": {},
      "type": "object"
    },
    "required": {
      "items": {
        "type": "string"
      },
      "type": "array"
    },
    "items": {
      "$ref": "#/components/schemas/IInputValidationSchema"
    }
  },
  "required": [
    "type"
  ],
  "type": "object",
  "additionalProperties": {}
}
```

