# Connection Schemas

> **Context for AI:** Connection configurations for external databases and services.
> These define how CML connects to various data stores.
> Connections are typically referenced by auth configurations and used by input/output clients.
>
> **Supported connections:**
> - **FTP/SFTP**: File transfer connections
> - **Snowflake**: Snowflake data warehouse
> - **PostgreSQL**: PostgreSQL databases
> - **MSSQL**: Microsoft SQL Server
> - **ClickHouse**: ClickHouse analytical database
>
> **See also:** [Auth types](auth/auth-types.md), [Input clients](input-clients/), [Output clients](output-clients/)

### `IFtpConnection`

DEPRECATED
Use authId instead.

Defines connection details.

```json
{
  "description": "DEPRECATED\nUse authId instead.\n\nDefines connection details.",
  "properties": {
    "host": {
      "type": "string",
      "description": "Defines host connection.",
      "example": "my.ftp.server.com"
    },
    "user": {
      "type": "string",
      "description": "User with access to given host.",
      "example": "myUserName"
    },
    "password": {
      "type": "string",
      "description": "User`s password.",
      "example": "my-super-secret-pass"
    },
    "secure": {
      "type": "boolean",
      "nullable": true,
      "description": "Flag if fpt use secure connection or not. It depends on given host.",
      "example": "false"
    }
  },
  "required": [
    "host",
    "user",
    "password",
    "secure"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ISnowflakeConnection`

DEPRECATED
Use authId instead.

Defines connection details.

```json
{
  "description": "DEPRECATED\nUse authId instead.\n\nDefines connection details.",
  "properties": {
    "account": {
      "type": "string",
      "description": "Defines account connection.",
      "example": "my.europe-west4.gcp"
    },
    "database": {
      "type": "string",
      "description": "Name of DB in Snowflake.",
      "example": "MY_DB"
    },
    "username": {
      "type": "string",
      "description": "User with access to given account.",
      "example": "myUserName"
    },
    "password": {
      "type": "string",
      "description": "User`s password.",
      "example": "my-super-secret-pass"
    },
    "role": {
      "type": "string",
      "description": "Optional attribute (but we recommend using this attribute). User`s role to be used to access data. If the attribute is present, the given role is used otherwise, the default\nusers'\nrole is used.\nThe default role might not have access to given resource or the default role can be changed in te future.",
      "example": "data_reader_role"
    },
    "schema": {
      "type": "string",
      "description": "Snowflake`s schema name.",
      "example": "MY_SCHEMA"
    },
    "warehouse": {
      "type": "string",
      "description": "Name of warehouse to be used to execute given statement.",
      "example": "COMPUTE_WH"
    }
  },
  "required": [
    "account",
    "database",
    "username",
    "password",
    "schema",
    "warehouse"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IAdminSnowflakeConnection`

DEPRECATED
Use adminAuthId instead.

Defines connection details for admin access to Snowflake.

```json
{
  "description": "DEPRECATED\nUse adminAuthId instead.\n\nDefines connection details for admin access to Snowflake.",
  "properties": {
    "account": {
      "type": "string",
      "description": "Defines account connection.",
      "example": "my.europe-west4.gcp"
    },
    "database": {
      "type": "string",
      "description": "Name of DB in Snowflake.",
      "example": "MY_DB"
    },
    "username": {
      "type": "string",
      "description": "User with access to given account.",
      "example": "myUserName"
    },
    "password": {
      "type": "string",
      "description": "User`s password.",
      "example": "my-super-secret-pass"
    },
    "role": {
      "type": "string",
      "description": "Optional attribute (but we recommend using this attribute). User`s role to be used to access data. If the attribute is present, the given role is used otherwise, the default\nusers'\nrole is used.\nThe default role might not have access to given resource or the default role can be changed in te future.",
      "example": "data_reader_role"
    },
    "schema": {
      "type": "string",
      "description": "Snowflake`s schema name.",
      "example": "MY_SCHEMA"
    },
    "warehouse": {
      "type": "string",
      "description": "Name of warehouse to be used to execute given statement.",
      "example": "COMPUTE_WH"
    },
    "overrideBucket": {
      "type": "string",
      "description": "Possibility to override bucket name to customize integration between GCP and Snowflake.",
      "example": "cmk-prod"
    },
    "storageIntegration": {
      "type": "string",
      "description": "Possibility to override storage integration name to customize integration between GCP and Snowflake.\ndefault value `dl_cml_gcp_storage`",
      "example": "dl_cml_gcp_storage"
    }
  },
  "required": [
    "account",
    "database",
    "username",
    "password",
    "schema",
    "warehouse"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IPgConnection`

```json
{
  "properties": {
    "host": {
      "type": "string"
    },
    "port": {
      "type": "number",
      "format": "double"
    },
    "database": {
      "type": "string"
    },
    "username": {
      "type": "string"
    },
    "password": {
      "type": "string"
    },
    "ssl": {
      "anyOf": [
        {
          "type": "boolean"
        },
        {
          "additionalProperties": false,
          "type": "object"
        }
      ]
    },
    "max": {
      "type": "number",
      "format": "double"
    },
    "idleTimeoutMillis": {
      "type": "number",
      "format": "double"
    },
    "connectionTimeoutMillis": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "host",
    "port",
    "database",
    "username",
    "password"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IMssqlConnection`

```json
{
  "properties": {
    "server": {
      "type": "string",
      "description": "Server hostname or IP address."
    },
    "database": {
      "type": "string",
      "description": "Database name.",
      "example": "cml_test_db"
    },
    "user": {
      "type": "string",
      "description": "Username for authentication.",
      "example": "cml_test_user"
    },
    "password": {
      "type": "string",
      "description": "Password for authentication.",
      "example": "TestPassword123!"
    },
    "port": {
      "type": "number",
      "format": "double",
      "description": "Server port. Defaults to 1433 if not specified.",
      "example": 1433
    },
    "schema": {
      "type": "string",
      "description": "Database schema name\nIn MSSQL, tables are typically referenced as schema.table or database.schema.table."
    },
    "options": {
      "properties": {
        "instanceName": {
          "type": "string",
          "description": "Instance name (optional).",
          "example": "SQLEXPRESS"
        },
        "trustServerCertificate": {
          "type": "boolean",
          "description": "Trust server certificate. Required when encrypt is false.",
          "example": true
        },
        "encrypt": {
          "type": "boolean",
          "description": "Enable encryption. Defaults to false for local connections.",
          "example": false
        }
      },
      "type": "object",
      "description": "Additional connection options."
    },
    "pool": {
      "properties": {
        "idleTimeoutMillis": {
          "type": "number",
          "format": "double",
          "description": "Idle timeout in milliseconds after which an unused connection is closed.",
          "example": 30000
        },
        "min": {
          "type": "number",
          "format": "double",
          "description": "Minimum number of connections to keep in the pool.",
          "example": 0
        },
        "max": {
          "type": "number",
          "format": "double",
          "description": "Maximum number of connections in the pool.",
          "example": 10
        }
      },
      "required": [
        "idleTimeoutMillis",
        "min",
        "max"
      ],
      "type": "object",
      "description": "Connection pool settings. Optional; defaults are applied by the connector if omitted."
    }
  },
  "required": [
    "server",
    "database",
    "user",
    "password"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IClickHouseConnection`

ClickHouse connection configuration.

```json
{
  "description": "ClickHouse connection configuration.",
  "properties": {
    "url": {
      "type": "string",
      "description": "ClickHouse HTTP or native connection URL."
    },
    "username": {
      "type": "string",
      "description": "ClickHouse username."
    },
    "password": {
      "type": "string",
      "description": "ClickHouse password."
    },
    "database": {
      "type": "string",
      "description": "Target ClickHouse database name.\n\nMust be a valid ClickHouse identifier.",
      "example": "default",
      "pattern": "^[a-zA-Z_][a-zA-Z0-9_]*$",
      "minLength": 1
    }
  },
  "required": [
    "url",
    "username",
    "password",
    "database"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ConnectionOptions`

```json
{
  "properties": {
    "username": {
      "type": "string"
    },
    "password": {
      "type": "string"
    },
    "role": {
      "type": "string"
    }
  },
  "required": [
    "username",
    "password",
    "role"
  ],
  "type": "object",
  "additionalProperties": false
}
```

