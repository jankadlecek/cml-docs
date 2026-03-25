# Output Client Shared Types

> **Context for AI:** Shared types for all output clients. This includes:
> - **OutputClientTypeEnum**: Discriminator enum for all output client types
> - **Output handlers**: How to format output data (JSON, CSV, XML, EDI)
> - **Result hooks**: Post-processing hooks that run after output completes
> - **Action config**: How output actions are configured in orchestrations
>
> Each output client type has its own file in this folder with detailed schema.
>
> **See also:** Individual output client files in this folder

### `OutputClientTypeEnum.FTP`

```json
{
  "enum": [
    "FTP"
  ],
  "type": "string"
}
```

### `IFtpDynamicNameParam`

```json
{
  "properties": {
    "name": {
      "type": "string"
    },
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam"
    }
  },
  "required": [
    "name",
    "contextParam"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IJsonOutputHandler`

Handler attributes to convert data from JS object(s) format to Json.

```json
{
  "description": "Handler attributes to convert data from JS object(s) format to Json.",
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

### `IXmlOutputHandler`

Handler attributes to convert from JS object(s) format to XML.
For XML conversion we use [xml2js package](https://www.npmjs.com/package/xml2js).

```json
{
  "description": "Handler attributes to convert from JS object(s) format to XML.\nFor XML conversion we use [xml2js package](https://www.npmjs.com/package/xml2js).",
  "properties": {
    "encoding": {
      "type": "string",
      "description": "Encoding to by used for inputHandler / outputHandler communication.\nUTF-8 is used by default.",
      "example": "windows-1250"
    },
    "rootObjectName": {
      "type": "string",
      "description": "If defined data are encapsulated to this attribute.\n```\n<order> ... data ... </order>\n```",
      "example": "order"
    }
  },
  "type": "object",
  "additionalProperties": false
}
```

### `ICsvOutputHandler`

Provides CSV format conversion from JS object(s) to CSV. For more information please refer to [FastCSV](https://www.c2fo.io/fast-csv/docs/parsing/options).

```json
{
  "description": "Provides CSV format conversion from JS object(s) to CSV. For more information please refer to [FastCSV](https://www.c2fo.io/fast-csv/docs/parsing/options).",
  "properties": {
    "encoding": {
      "type": "string",
      "description": "Encoding to by used for inputHandler / outputHandler communication.\nUTF-8 is used by default.",
      "example": "windows-1250"
    },
    "delimiter": {
      "type": "string"
    },
    "rowDelimiter": {
      "type": "string"
    },
    "quote": {
      "anyOf": [
        {
          "type": "string"
        },
        {
          "type": "boolean"
        }
      ]
    },
    "escape": {
      "type": "string"
    },
    "quoteColumns": {
      "type": "boolean"
    },
    "quoteHeaders": {
      "type": "boolean"
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
      ],
      "nullable": true
    },
    "writeHeaders": {
      "type": "boolean"
    },
    "includeEndRowDelimiter": {
      "type": "boolean"
    },
    "alwaysWriteHeaders": {
      "type": "boolean"
    }
  },
  "type": "object",
  "additionalProperties": false
}
```

### `IEdiSegment`

```json
{
  "properties": {
    "attributeName": {
      "type": "string",
      "description": "name of the attribute in the ADF {@link ActionFlowDataItem }"
    },
    "repeatable": {
      "type": "boolean"
    }
  },
  "required": [
    "attributeName",
    "repeatable"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IEdiOutputHandler`

Provides Edi format conversion from JS object(s) to Edi file.
The EDI file has lots of variants. First of all we are handling [Ordering process](https://api.stanleystella.com/page/edi-userguide)

Definitions:
- Elements – The smallest part of a message, providing submitted values (e.g. “50” or “KGM” or “Potatoes”)
- Segments – A collection of elements or values logically combined to provide an information (e.g. Quantity 50 kilograms of potatoes = part of )
- Transaction sets – A collection of segments, composing a message (e.g. an Invoice for the sale of 50 kilogram of potatoes = whole order)

CAUTION: because we are using currently only on key for segment is supported in data part.

Additional formating is not supported yet, but it is possible to use Mapper to format data as needed - e.g. number or date formating

Example:
Prepared data after Mapping
```
[{
    header: {
        '0001': '*** HEADER ***',
        '0101': 'R00000',
        '0114': 'MY REFERENCE',
    },
    headerEnd: {
        '0001': '*** HEADER ***'
    },
    shipTo: {
        '0001': '*** TO BE SHIPPED TO ***',
        '0602': 'SPECIFIC',
        '0603': 'John Doe',
        '0604': 'Some_adress',
        '0605': 'SecondLine',
        '0606': '9999',
        '0607': 'Some_City',
        '0608': 'BE',
        '0609': 'John Doe Jr.',
        '0610': '0011234567',
        '0611': 'john.doe@aol.com',
    },
    preLinesComment: {
        '0001': '*************'
    },
    lines: [
        {
            '0001': '*** LINES ***',
            '0501': 'STSM522',
            '0502': 'C0031M',
            '0503': '2',
        },
        {
            '0001': '*** LINES ***',
            '0501': 'STSM522',
            '0502': 'C0021M',
            '0503': '4',
        }
    ],
    messageEnd: {
        '9999': '*end of order*'
    }
}]
```

EDI file Configuration:
```
{
    elementsDelimiter: '\n',
    segmentsDelimiter: '\n',
    transactionSetDelimiter: '',
    keysDelimiter: '-',
    segments: [
        {
            attributeName: 'header',
            repeatable: false
        },
        {
            attributeName: 'headerEnd',
            repeatable: false
        },
        {
            attributeName: 'shipTo',
            repeatable: false
        },
        {
            attributeName: 'preLinesComment',
            repeatable: false,
        },
        {
            attributeName: 'lines',
            repeatable: true,
        },
        {
            attributeName: 'messageEnd',
            repeatable: false,
        }
    ]
}
```

Expected result
```
0001-*** HEADER ***
0101-R00000
0114-MY REFERENCE
0001-*** HEADER ***
0001-*** TO BE SHIPPED TO ***
0602-SPECIFIC
0603-John Doe
0604-Some_adress
0605-SecondLine
0606-9999
0607-Some_City
0608-BE
0609-John Doe Jr.
0610-0011234567
0611-john.doe@aol.com
0001-*************
0001-*** LINES ***
0501-STSM522
0502-C0031M
0503-2
0001-*** LINES ***
0501-STSM522
0502-C0021M
0503-4
9999-*end of order*
```

```json
{
  "description": "Provides Edi format conversion from JS object(s) to Edi file.\nThe EDI file has lots of variants. First of all we are handling [Ordering process](https://api.stanleystella.com/page/edi-userguide)\n\nDefinitions:\n- Elements \u2013 The smallest part of a message, providing submitted values (e.g. \u201c50\u201d or \u201cKGM\u201d or \u201cPotatoes\u201d)\n- Segments \u2013 A collection of elements or values logically combined to provide an information (e.g. Quantity 50 kilograms of potatoes = part of )\n- Transaction sets \u2013 A collection of segments, composing a message (e.g. an Invoice for the sale of 50 kilogram of potatoes = whole order)\n\nCAUTION: because we are using currently only on key for segment is supported in data part.\n\nAdditional formating is not supported yet, but it is possible to use Mapper to format data as needed - e.g. number or date formating\n\nExample:\nPrepared data after Mapping\n```\n[{\n    header: {\n        '0001': '*** HEADER ***',\n        '0101': 'R00000',\n        '0114': 'MY REFERENCE',\n    },\n    headerEnd: {\n        '0001': '*** HEADER ***'\n    },\n    shipTo: {\n        '0001': '*** TO BE SHIPPED TO ***',\n        '0602': 'SPECIFIC',\n        '0603': 'John Doe',\n        '0604': 'Some_adress',\n        '0605': 'SecondLine',\n        '0606': '9999',\n        '0607': 'Some_City',\n        '0608': 'BE',\n        '0609': 'John Doe Jr.',\n        '0610': '0011234567',\n        '0611': 'john.doe@aol.com',\n    },\n    preLinesComment: {\n        '0001': '*************'\n    },\n    lines: [\n        {\n            '0001': '*** LINES ***',\n            '0501': 'STSM522',\n            '0502': 'C0031M',\n            '0503': '2',\n        },\n        {\n            '0001': '*** LINES ***',\n            '0501': 'STSM522',\n            '0502': 'C0021M',\n            '0503': '4',\n        }\n    ],\n    messageEnd: {\n        '9999': '*end of order*'\n    }\n}]\n```\n\nEDI file Configuration:\n```\n{\n    elementsDelimiter: '\\n',\n    segmentsDelimiter: '\\n',\n    transactionSetDelimiter: '',\n    keysDelimiter: '-',\n    segments: [\n        {\n            attributeName: 'header',\n            repeatable: false\n        },\n        {\n            attributeName: 'headerEnd',\n            repeatable: false\n        },\n        {\n            attributeName: 'shipTo',\n            repeatable: false\n        },\n        {\n            attributeName: 'preLinesComment',\n            repeatable: false,\n        },\n        {\n            attributeName: 'lines',\n            repeatable: true,\n        },\n        {\n            attributeName: 'messageEnd',\n            repeatable: false,\n        }\n    ]\n}\n```\n\nExpected result\n```\n0001-*** HEADER ***\n0101-R00000\n0114-MY REFERENCE\n0001-*** HEADER ***\n0001-*** TO BE SHIPPED TO ***\n0602-SPECIFIC\n0603-John Doe\n0604-Some_adress\n0605-SecondLine\n0606-9999\n0607-Some_City\n0608-BE\n0609-John Doe Jr.\n0610-0011234567\n0611-john.doe@aol.com\n0001-*************\n0001-*** LINES ***\n0501-STSM522\n0502-C0031M\n0503-2\n0001-*** LINES ***\n0501-STSM522\n0502-C0021M\n0503-4\n9999-*end of order*\n```",
  "properties": {
    "encoding": {
      "type": "string",
      "description": "Encoding to by used for inputHandler / outputHandler communication.\nUTF-8 is used by default.",
      "example": "windows-1250"
    },
    "keysDelimiter": {
      "type": "string",
      "description": "Used to delimit the field (key in JSON) and value (value in JSON).\n\nExample: `-`\nkey: value -> key-value"
    },
    "elementsDelimiter": {
      "type": "string",
      "description": "To use non-printable characters use:\n- Line Feed (New line) - `\\n`\n- Horizontal Tab - `\\t`"
    },
    "segmentsDelimiter": {
      "type": "string",
      "description": "To use non-printable characters use:\n- Line Feed (New line) - `\\n`\n- Horizontal Tab - `\\t`"
    },
    "transactionSetDelimiter": {
      "type": "string",
      "description": "To use non-printable characters use:\n- Line Feed (New line) - `\\n`\n- Horizontal Tab - `\\t`"
    },
    "segments": {
      "items": {
        "$ref": "#/components/schemas/IEdiSegment"
      },
      "type": "array"
    }
  },
  "required": [
    "keysDelimiter",
    "elementsDelimiter",
    "segmentsDelimiter",
    "transactionSetDelimiter",
    "segments"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IConvertOutputHandler`

Common Converter Output Handler

```json
{
  "description": "Common Converter Output Handler",
  "properties": {
    "config": {
      "anyOf": [
        {
          "$ref": "#/components/schemas/IJsonOutputHandler"
        },
        {
          "$ref": "#/components/schemas/IXmlOutputHandler"
        },
        {
          "$ref": "#/components/schemas/ICsvOutputHandler"
        },
        {
          "$ref": "#/components/schemas/IEdiOutputHandler"
        }
      ],
      "description": "Required attribute. Contains appropriate output handler configuration for given type."
    },
    "type": {
      "$ref": "#/components/schemas/ResponseHandlerType",
      "description": "Required attribute. Defines type of the response handler"
    }
  },
  "required": [
    "type"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `OutputClientTypeEnum.GOOGLE_CLOUD_STORAGE`

```json
{
  "enum": [
    "GOOGLE_CLOUD_STORAGE"
  ],
  "type": "string"
}
```

### `OutputClientTypeEnum.G_BIG_QUERY_LOADER`

```json
{
  "enum": [
    "G_BIG_QUERY_LOADER"
  ],
  "type": "string"
}
```

### `OutputClientTypeEnum.FACEBOOK_CUSTOM_AUDIENCE`

```json
{
  "enum": [
    "FACEBOOK_CUSTOM_AUDIENCE"
  ],
  "type": "string"
}
```

### `DynamicContextValueType`

```json
{
  "properties": {
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam",
      "description": "If value is not preset the context value is determined from the context."
    },
    "value": {
      "type": "string",
      "description": "if present tha value is taken instead of context param evaluation"
    }
  },
  "required": [
    "contextParam"
  ],
  "type": "object"
}
```

### `OutputClientTypeEnum.HTTP`

```json
{
  "enum": [
    "HTTP"
  ],
  "type": "string"
}
```

### `OutputClientTypeEnum.SINGLE_HTTP`

```json
{
  "enum": [
    "SINGLE_HTTP"
  ],
  "type": "string"
}
```

### `OutputClientTypeEnum.SNOWFLAKE`

```json
{
  "enum": [
    "SNOWFLAKE"
  ],
  "type": "string"
}
```

### `OutputClientTypeEnum.POSTGRES`

```json
{
  "enum": [
    "POSTGRES"
  ],
  "type": "string"
}
```

### `OutputClientTypeEnum.CLICKHOUSE`

```json
{
  "enum": [
    "CLICKHOUSE"
  ],
  "type": "string"
}
```

### `IOutputActionConfig`

```json
{
  "allOf": [
    {
      "properties": {
        "skipIfEmpty": {
          "type": "boolean",
          "description": "If true, the action will be skipped if the action flow data under the specific mapping is effectively empty.\nEmpty data is defined as: null, undefined, {}, [] and [undefined/null].\n\nUse when it cannot be guaranteed that the action flow data under the specific mapping will be generated.\nThis usually happens when Input action does not generate any data."
        },
        "name": {
          "type": "string"
        }
      },
      "required": [
        "name"
      ],
      "type": "object"
    },
    {
      "anyOf": [
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IFtpOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.FTP"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IGCSOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.GOOGLE_CLOUD_STORAGE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IGoogleBQOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.G_BIG_QUERY_LOADER"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IFacebookCustomAudienceOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.FACEBOOK_CUSTOM_AUDIENCE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IHttpOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.HTTP"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/ISingleHttpOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.SINGLE_HTTP"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/ISnowflakeOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.SNOWFLAKE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IHttpOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.RESPONSE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IPgOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.POSTGRES"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/IClickHouseOutputClient"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.CLICKHOUSE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        }
      ]
    }
  ]
}
```

### `IOutputActionConfigWithoutResultHook`

```json
{
  "allOf": [
    {
      "$ref": "#/components/schemas/IOutputActionConfig"
    },
    {
      "anyOf": [
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_IFtpOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.FTP"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_IGCSOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.GOOGLE_CLOUD_STORAGE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_IGoogleBQOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.G_BIG_QUERY_LOADER"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_IFacebookCustomAudienceOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.FACEBOOK_CUSTOM_AUDIENCE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_IHttpOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.HTTP"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_ISingleHttpOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.SINGLE_HTTP"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_ISnowflakeOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.SNOWFLAKE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        },
        {
          "properties": {
            "client": {
              "$ref": "#/components/schemas/Omit_IHttpOutputClient.keyofIClientWithResultHook_"
            },
            "type": {
              "$ref": "#/components/schemas/OutputClientTypeEnum.RESPONSE"
            }
          },
          "required": [
            "client",
            "type"
          ],
          "type": "object"
        }
      ]
    }
  ]
}
```

### `OutputClientTypeEnum`

```json
{
  "enum": [
    "RESPONSE",
    "G_BIG_QUERY_LOADER",
    "SINGLE_HTTP",
    "HTTP",
    "FACEBOOK_CUSTOM_AUDIENCE",
    "FTP",
    "GOOGLE_CLOUD_STORAGE",
    "SNOWFLAKE",
    "POSTGRES",
    "CLICKHOUSE"
  ],
  "type": "string"
}
```

### `IResultHookDetail`

```json
{
  "properties": {
    "type": {
      "$ref": "#/components/schemas/ResultHookTypeEnum"
    },
    "status": {
      "$ref": "#/components/schemas/ResultHookStatusEnum"
    },
    "error": {
      "type": "string"
    },
    "outputClientName": {
      "type": "string"
    },
    "outputClientType": {
      "$ref": "#/components/schemas/OutputClientTypeEnum"
    }
  },
  "required": [
    "type",
    "status",
    "outputClientName",
    "outputClientType"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IResultHookInfo`

```json
{
  "properties": {
    "hooks": {
      "items": {
        "$ref": "#/components/schemas/IResultHookDetail"
      },
      "type": "array"
    }
  },
  "required": [
    "hooks"
  ],
  "type": "object",
  "additionalProperties": false
}
```

