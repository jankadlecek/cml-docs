# HTTP Output Client

> **Context for AI:** Sends data to REST APIs via HTTP requests.
> Two variants exist:
> - **IHttpOutputClient**: Standard HTTP output, sends each data item as a separate request
> - **ISingleHttpOutputClient**: Sends all data in a single request with data mapping
>
> Supports retries, request throttling (wait), result hooks, and custom data mapping.
>
> **See also:** [Shared output types](_types.md), [Auth types](../auth/auth-types.md)

### `IRequestWait`

```json
{
  "properties": {
    "defaultInMs": {
      "type": "integer",
      "format": "int32",
      "nullable": true,
      "description": "This attribute provides possibility to wait for exact milliseconds.\nExample:\n```\n10 => 10ms\n250 => 250ms\n5000 => 5000ms = 5s\n```",
      "example": "250",
      "maximum": 5000
    }
  },
  "required": [
    "defaultInMs"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IHttpResultHookClientConfig`

```json
{
  "properties": {
    "type": {
      "$ref": "#/components/schemas/ResultHookTypeEnum"
    },
    "mapper": {
      "allOf": [
        {
          "$ref": "#/components/schemas/IMapperConfig"
        }
      ],
      "nullable": true,
      "description": "Non-required - if not provided the mapper is not used"
    },
    "outputClient": {
      "$ref": "#/components/schemas/IOutputActionConfigWithoutResultHook"
    },
    "stopExecutionOnHookError": {
      "type": "boolean",
      "description": "If true, SUCCESS type hooks with error will result in the entire action ending with error.\n\n- true -> the process will be stopped and entire action will end with error.\n- false -> the process will continue and the error will be only logged.\n\nUse if receiving the hook is critical to the entire action."
    }
  },
  "required": [
    "type",
    "outputClient"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IHttpRetryable`

Defines all possible attributes to be used with Clients which uses http for communication.

```json
{
  "description": "Defines all possible attributes to be used with Clients which uses http for communication.",
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
    },
    "forErrorCodes": {
      "items": {
        "type": "number",
        "format": "double"
      },
      "type": "array",
      "nullable": true,
      "description": "Defines list of error codes. If define only for these error codes retry logic is applied. For other codes the retry logic is skipped.\nIf not set this condition is not applied.",
      "minItems": 1
    },
    "retryHeader": {
      "type": "string",
      "description": "Defines name of the header to use as the delay between requests.",
      "example": "Retry-After"
    }
  },
  "required": [
    "maxRetries"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IHttpOutputClient`

```json
{
  "properties": {
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler"
    },
    "retryable": {
      "$ref": "#/components/schemas/IHttpRetryable"
    },
    "type": {
      "$ref": "#/components/schemas/ExtractionConfigTypeEnum"
    },
    "method": {
      "type": "string"
    },
    "url": {
      "type": "string"
    },
    "pathParams": {
      "$ref": "#/components/schemas/HttpPathParams"
    },
    "queryParams": {
      "$ref": "#/components/schemas/HttpQueryParams"
    },
    "headers": {
      "$ref": "#/components/schemas/HttpHeaders"
    },
    "dynamicParams": {
      "items": {
        "$ref": "#/components/schemas/DynamicParamType"
      },
      "type": "array",
      "description": "Provides possibility to add dynamic params to given request.\nRecommended way to add auth token to the request."
    },
    "bodyTemplate": {
      "type": "string"
    },
    "dynamicParamName": {
      "type": "string",
      "description": "DEPRECATED\nUse dynamicParams of type BODY instead.\nIn case of conflict, result from dynamicParams will be used instead."
    },
    "requestLimit": {
      "type": "number",
      "format": "double"
    },
    "authId": {
      "type": "string",
      "description": "Deprecated. Use dynamicParams of type HEADER in combination with authId attribute instead.\nIn case of conflict, result from dynamicParams will be used instead.\n\nAuthorization's ID generated by `/api/rest/v1/components/auths` api endpoint.\nif present CML automatically generates authorization tokens and append to the request.",
      "example": "some-uuid-v4"
    },
    "timeout": {
      "type": "number",
      "format": "double"
    },
    "continueOnLastRequestFailure": {
      "type": "boolean",
      "description": "hidden\nDEPRECATED"
    },
    "continueOnRequestFailure": {
      "type": "boolean",
      "description": "This attribute provides to continue to write another reqeust in case of error for last request). This attribute should be used only with request limit. Without request\nlimit this attribute has no impact to write result."
    },
    "preRequestWait": {
      "$ref": "#/components/schemas/IRequestWait",
      "description": "This attribute provides possibility to wait before request is executed."
    },
    "postRequestWait": {
      "$ref": "#/components/schemas/IRequestWait",
      "description": "This attribute provides possibility to wait after request is executed."
    },
    "resultHookClient": {
      "items": {
        "$ref": "#/components/schemas/IHttpResultHookClientConfig"
      },
      "type": "array",
      "description": "Result hook client - same as IClientWithResultHook"
    }
  },
  "required": [
    "handler",
    "method",
    "url"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IRequestDataParamMapping`

Provides possibility to define request mapping from Action Data Flow Item to Reqeust.

if source is applied => is used
if contextParam is applied => is used
if source and context param applied => context param is used
if source and context not applied => an error is thrown.

```json
{
  "properties": {
    "contextParam": {
      "$ref": "#/components/schemas/ContextParam",
      "description": "If present value is set to context param."
    },
    "name": {
      "type": "string",
      "description": "Name of attribute to be replaced.",
      "example": "$ORDER_ID$"
    },
    "source": {
      "type": "string",
      "description": "Attribute name which data should be taken from Action Flow Data Item.",
      "example": "orderId"
    }
  },
  "required": [
    "name"
  ],
  "type": "object",
  "description": "Provides possibility to define request mapping from Action Data Flow Item to Reqeust.\n\nif source is applied => is used\nif contextParam is applied => is used\nif source and context param applied => context param is used\nif source and context not applied => an error is thrown."
}
```

### `IHttpRequestDataMapper`

Provides possibility to assemble request from given Action Flow Data Item.

Action Flow Data Item example:

```
{
    orderId: 1234,
    orderItem: {
        "itemId": 3456,
        "itemName": "Millennium Falcon",
        "count": 10,
        "amount": 5000
    }
}
```

```json
{
  "description": "Provides possibility to assemble request from given Action Flow Data Item.\n\nAction Flow Data Item example:\n\n```\n{\n    orderId: 1234,\n    orderItem: {\n        \"itemId\": 3456,\n        \"itemName\": \"Millennium Falcon\",\n        \"count\": 10,\n        \"amount\": 5000\n    }\n}\n```",
  "properties": {
    "bodyPathSource": {
      "type": "string",
      "description": "Optional param used to specify which data should be used as body.\n\nOnly first level attribute's names are supported.\n\nExample:\n`\"pathToBodyAttribute\": \"orderItem\"`\nBody output\n```\norderItem:\n     \"itemId\": 3456,\n     \"itemName\": \"Millennium Falcon\",\n     \"count\": 10,\n     \"amount\": 5000\n}\n```"
    },
    "headerParams": {
      "items": {
        "$ref": "#/components/schemas/IRequestDataParamMapping"
      },
      "type": "array",
      "description": "Params to be substituted in request header."
    },
    "pathParams": {
      "items": {
        "$ref": "#/components/schemas/IRequestDataParamMapping"
      },
      "type": "array",
      "description": "Params to be substituted in request url path."
    },
    "queryParams": {
      "items": {
        "$ref": "#/components/schemas/IRequestDataParamMapping"
      },
      "type": "array",
      "description": "Params to be substituted in request url query."
    }
  },
  "type": "object",
  "additionalProperties": false
}
```

### `ISingleHttpOutputClient`

```json
{
  "properties": {
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler"
    },
    "retryable": {
      "$ref": "#/components/schemas/IHttpRetryable"
    },
    "authId": {
      "type": "string",
      "description": "Deprecated. Use dynamicParams of type HEADER in combination with authId attribute instead.\nIn case of conflict, result from dynamicParams will be used instead.\n\nAuthorization's ID generated by `/api/rest/v1/components/auths` api endpoint.\nif present CML automatically generates authorization tokens and append to the request.",
      "example": "some-uuid-v4"
    },
    "headers": {
      "$ref": "#/components/schemas/HttpHeaders",
      "description": "Deprecated. Use dynamicParams of type HEADER instead.\nIn case of conflict, result from dynamicParams will be used instead.\n\nProvides possibility to add static headers to given request."
    },
    "method": {
      "type": "string",
      "description": "HTTP Method for given url. For all possible values please visit `https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods`",
      "example": "POST"
    },
    "requestDataMapper": {
      "$ref": "#/components/schemas/IHttpRequestDataMapper",
      "description": "Provides possibility to assemble request from given Action Flow Data Item."
    },
    "queryParams": {
      "$ref": "#/components/schemas/HttpQueryParams",
      "description": "Provides possibility to add static headers to given request."
    },
    "dynamicParams": {
      "items": {
        "$ref": "#/components/schemas/DynamicParamTypeWithoutBody"
      },
      "type": "array",
      "description": "Provides possibility to add dynamic params to given request.\nRecommended way to add auth token to the request."
    },
    "timeout": {
      "type": "number",
      "format": "double",
      "description": "Optional param used to define user specific request timeout.\n\nDefault value is: `10 min`"
    },
    "url": {
      "type": "string",
      "description": "Resource url.",
      "example": "{{cml_url}}/api/rest/v1/playground/mock/orders"
    },
    "resultHookClient": {
      "items": {
        "$ref": "#/components/schemas/IHttpResultHookClientConfig"
      },
      "type": "array",
      "description": "Result hook client - same as IClientWithResultHook"
    }
  },
  "required": [
    "handler",
    "method",
    "url"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `Pick_IHttpOutputClient.Exclude_keyofIHttpOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "authId": {
      "type": "string",
      "description": "Deprecated. Use dynamicParams of type HEADER in combination with authId attribute instead.\nIn case of conflict, result from dynamicParams will be used instead.\n\nAuthorization's ID generated by `/api/rest/v1/components/auths` api endpoint.\nif present CML automatically generates authorization tokens and append to the request.",
      "example": "some-uuid-v4"
    },
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler"
    },
    "retryable": {
      "$ref": "#/components/schemas/IHttpRetryable"
    },
    "type": {
      "$ref": "#/components/schemas/ExtractionConfigTypeEnum"
    },
    "method": {
      "type": "string"
    },
    "url": {
      "type": "string"
    },
    "pathParams": {
      "$ref": "#/components/schemas/HttpPathParams"
    },
    "queryParams": {
      "$ref": "#/components/schemas/HttpQueryParams"
    },
    "headers": {
      "$ref": "#/components/schemas/HttpHeaders"
    },
    "dynamicParams": {
      "items": {
        "$ref": "#/components/schemas/DynamicParamType"
      },
      "type": "array",
      "description": "Provides possibility to add dynamic params to given request.\nRecommended way to add auth token to the request."
    },
    "bodyTemplate": {
      "type": "string"
    },
    "dynamicParamName": {
      "type": "string",
      "description": "DEPRECATED\nUse dynamicParams of type BODY instead.\nIn case of conflict, result from dynamicParams will be used instead."
    },
    "requestLimit": {
      "type": "number",
      "format": "double"
    },
    "timeout": {
      "type": "number",
      "format": "double"
    },
    "continueOnLastRequestFailure": {
      "type": "boolean",
      "description": "hidden\nDEPRECATED"
    },
    "continueOnRequestFailure": {
      "type": "boolean",
      "description": "This attribute provides to continue to write another reqeust in case of error for last request). This attribute should be used only with request limit. Without request\nlimit this attribute has no impact to write result."
    },
    "preRequestWait": {
      "$ref": "#/components/schemas/IRequestWait",
      "description": "This attribute provides possibility to wait before request is executed."
    },
    "postRequestWait": {
      "$ref": "#/components/schemas/IRequestWait",
      "description": "This attribute provides possibility to wait after request is executed."
    }
  },
  "required": [
    "handler",
    "method",
    "url"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_IHttpOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_IHttpOutputClient.Exclude_keyofIHttpOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

### `Pick_ISingleHttpOutputClient.Exclude_keyofISingleHttpOutputClient.keyofIClientWithResultHook__`

From T, pick a set of properties whose keys are in the union K

```json
{
  "properties": {
    "authId": {
      "type": "string",
      "description": "Deprecated. Use dynamicParams of type HEADER in combination with authId attribute instead.\nIn case of conflict, result from dynamicParams will be used instead.\n\nAuthorization's ID generated by `/api/rest/v1/components/auths` api endpoint.\nif present CML automatically generates authorization tokens and append to the request.",
      "example": "some-uuid-v4"
    },
    "handler": {
      "$ref": "#/components/schemas/IConvertOutputHandler"
    },
    "retryable": {
      "$ref": "#/components/schemas/IHttpRetryable"
    },
    "method": {
      "type": "string",
      "description": "HTTP Method for given url. For all possible values please visit `https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods`",
      "example": "POST"
    },
    "url": {
      "type": "string",
      "description": "Resource url.",
      "example": "{{cml_url}}/api/rest/v1/playground/mock/orders"
    },
    "queryParams": {
      "$ref": "#/components/schemas/HttpQueryParams",
      "description": "Provides possibility to add static headers to given request."
    },
    "headers": {
      "$ref": "#/components/schemas/HttpHeaders",
      "description": "Deprecated. Use dynamicParams of type HEADER instead.\nIn case of conflict, result from dynamicParams will be used instead.\n\nProvides possibility to add static headers to given request."
    },
    "dynamicParams": {
      "items": {
        "$ref": "#/components/schemas/DynamicParamType"
      },
      "type": "array",
      "description": "Provides possibility to add dynamic params to given request.\nRecommended way to add auth token to the request."
    },
    "timeout": {
      "type": "number",
      "format": "double",
      "description": "Optional param used to define user specific request timeout.\n\nDefault value is: `10 min`"
    },
    "requestDataMapper": {
      "$ref": "#/components/schemas/IHttpRequestDataMapper",
      "description": "Provides possibility to assemble request from given Action Flow Data Item."
    }
  },
  "required": [
    "handler",
    "method",
    "url"
  ],
  "type": "object",
  "description": "From T, pick a set of properties whose keys are in the union K"
}
```

### `Omit_ISingleHttpOutputClient.keyofIClientWithResultHook_`

Construct a type with the properties of T except for those in type K.

```json
{
  "$ref": "#/components/schemas/Pick_ISingleHttpOutputClient.Exclude_keyofISingleHttpOutputClient.keyofIClientWithResultHook__",
  "description": "Construct a type with the properties of T except for those in type K."
}
```

