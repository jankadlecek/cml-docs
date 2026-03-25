# Transformation Engine Schemas

> **Context for AI:** Transformation engines are external data processing tools
> (like dbt) integrated into CML. They have profiles defining execution parameters.
>
> **See also:** [Transformation engines endpoints](../endpoints/transformation-engines.md), [Trigger actions](actions/trigger-action.md)

### `ITransformationEngineApi`

API DTO contains basic info.

```json
{
  "description": "API DTO contains basic info.",
  "properties": {
    "id": {
      "type": "string",
      "description": "Specifies id for CML - MIB DBT integration."
    }
  },
  "required": [
    "id"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ITransformationEngineListResponse`

API to display existing Transformation Engine Configuration

```json
{
  "description": "API to display existing Transformation Engine Configuration",
  "properties": {
    "count": {
      "type": "integer",
      "format": "int32"
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/ITransformationEngineApi"
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

### `IProfileConfigurationTransformationEngineAPi`

Please provide whole dbt yaml file as a JSON.
Snowflake yaml example:
```yaml
default:
  outputs:
    dev:
      type: snowflake
      account: my-snwf-account
      # User/password auth
      user: user_name
      password: user_password
      database: db_name
      warehouse: wh_name
      schema: schema_name
      threads: 1
      client_session_keep_alive: False
      connect_retries: 0 # default 0
      connect_timeout: 600 # default: 10
      retry_on_database_errors: False # default: false
      retry_all: False # default: false
  target: dev
```

converted json:
```json
{
     "default": {
         "target": "dev",
         "outputs": {
             "dev": {
                 "type": "snowflake",
                 "user": "user_name",
                 "schema": "schema_name",
                 "account": "my-snwf-account",
                 "threads": 1,
                 "database": "db_name",
                 "password": "user_password",
                 "retry_all": false,
                 "warehouse": "wh_name",
                 "connect_retries": 0,
                 "connect_timeout": 600,
                 "retry_on_database_errors": false,
                 "client_session_keep_alive": false
              }
          }
      }
  }
```

```json
{
  "description": "Please provide whole dbt yaml file as a JSON.\nSnowflake yaml example:\n```yaml\ndefault:\n  outputs:\n    dev:\n      type: snowflake\n      account: my-snwf-account\n      # User/password auth\n      user: user_name\n      password: user_password\n      database: db_name\n      warehouse: wh_name\n      schema: schema_name\n      threads: 1\n      client_session_keep_alive: False\n      connect_retries: 0 # default 0\n      connect_timeout: 600 # default: 10\n      retry_on_database_errors: False # default: false\n      retry_all: False # default: false\n  target: dev\n```\n\nconverted json:\n```json\n{\n     \"default\": {\n         \"target\": \"dev\",\n         \"outputs\": {\n             \"dev\": {\n                 \"type\": \"snowflake\",\n                 \"user\": \"user_name\",\n                 \"schema\": \"schema_name\",\n                 \"account\": \"my-snwf-account\",\n                 \"threads\": 1,\n                 \"database\": \"db_name\",\n                 \"password\": \"user_password\",\n                 \"retry_all\": false,\n                 \"warehouse\": \"wh_name\",\n                 \"connect_retries\": 0,\n                 \"connect_timeout\": 600,\n                 \"retry_on_database_errors\": false,\n                 \"client_session_keep_alive\": false\n              }\n          }\n      }\n  }\n```",
  "properties": {},
  "type": "object",
  "additionalProperties": {}
}
```

### `IProfileCreateTransformationEngineApi`

```json
{
  "properties": {
    "profileName": {
      "type": "string"
    },
    "configuration": {
      "$ref": "#/components/schemas/IProfileConfigurationTransformationEngineAPi"
    }
  },
  "required": [
    "profileName",
    "configuration"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IInitTransformationEngineBodyApi`

API DTO used for initialization TE for CML project.

```json
{
  "description": "API DTO used for initialization TE for CML project.",
  "properties": {
    "dbtProject": {
      "properties": {
        "gitSsh": {
          "type": "string",
          "description": "Required param. Contains ssh git address.",
          "example": "git@gitlab.com:marketing.bi/..../my-dbt-project.git"
        },
        "branchName": {
          "type": "string",
          "description": "Optional param. Contains branch name to be used as default branch for TE.\nIf not present - default git branch (master / main) is used.",
          "example": "stage"
        }
      },
      "required": [
        "gitSsh"
      ],
      "type": "object",
      "description": "Contains basic configuration for DBT Mib project."
    },
    "profiles": {
      "items": {
        "$ref": "#/components/schemas/IProfileCreateTransformationEngineApi"
      },
      "type": "array",
      "nullable": true,
      "description": "Optional param used to defined profiles for Transformation Engine. Set null value to skip profile creation.",
      "minLength": 1
    }
  },
  "required": [
    "dbtProject",
    "profiles"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IProfileTransformationEngineApi`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "profileName": {
      "type": "string"
    },
    "createdAt": {
      "type": "string"
    },
    "createdBy": {
      "type": "string"
    },
    "projectId": {
      "type": "string"
    },
    "transformationEngineId": {
      "type": "string"
    },
    "updatedAt": {
      "type": "string"
    },
    "updatedBy": {
      "type": "string"
    }
  },
  "required": [
    "id",
    "profileName",
    "createdAt",
    "createdBy",
    "projectId",
    "transformationEngineId",
    "updatedAt",
    "updatedBy"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ITransformationEngineProfilesListResponse`

API to display existing Transformation Engine Configuration

```json
{
  "description": "API to display existing Transformation Engine Configuration",
  "properties": {
    "count": {
      "type": "integer",
      "format": "int32"
    },
    "data": {
      "items": {
        "$ref": "#/components/schemas/IProfileTransformationEngineApi"
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

