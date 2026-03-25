# Authentication Credentials

> **Context for AI:** Types used for user authentication (login).
> `ICredentialsApi` is the login request body, `IUserAuthorizationTokenApi` is the response.
>
> **See also:** [Users endpoints](../../endpoints/users.md)

### `IUserAuthorizationTokenApi`

```json
{
  "properties": {
    "token": {
      "type": "string"
    }
  },
  "required": [
    "token"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `ICredentialsApi`

```json
{
  "properties": {
    "userName": {
      "type": "string"
    },
    "password": {
      "type": "string"
    }
  },
  "required": [
    "userName",
    "password"
  ],
  "type": "object",
  "additionalProperties": false
}
```

