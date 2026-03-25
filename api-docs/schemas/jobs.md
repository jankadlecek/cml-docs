# Job Schemas

> **Context for AI:** Jobs are the execution units created when an orchestration runs.
> Workers pick up jobs, execute them, and report back via health reports.
>
> **See also:** [Jobs endpoints](../endpoints/jobs.md), [Orchestrations](orchestrations.md)

### `IJobFinishApi`

```json
{
  "properties": {
    "actionId": {
      "type": "string"
    },
    "result": {
      "items": {},
      "type": "array"
    }
  },
  "required": [
    "actionId",
    "result"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `WorkerInfo`

```json
{
  "properties": {
    "id": {
      "type": "string"
    },
    "lastReport": {
      "type": "number",
      "format": "double"
    }
  },
  "required": [
    "id",
    "lastReport"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IJobStatus`

```json
{
  "properties": {
    "actionId": {
      "type": "string"
    },
    "jobId": {
      "type": "string"
    },
    "projectId": {
      "type": "string"
    },
    "orchestrationId": {
      "type": "string"
    },
    "currentActions": {
      "items": {
        "$ref": "#/components/schemas/IActionStatus"
      },
      "type": "array"
    },
    "completedActions": {
      "items": {
        "$ref": "#/components/schemas/IActionStatus"
      },
      "type": "array"
    }
  },
  "required": [
    "actionId",
    "jobId",
    "projectId",
    "orchestrationId",
    "currentActions",
    "completedActions"
  ],
  "type": "object",
  "additionalProperties": false
}
```

### `IJobHealthReport`

```json
{
  "properties": {
    "workerInfo": {
      "$ref": "#/components/schemas/WorkerInfo"
    },
    "jobs": {
      "properties": {},
      "additionalProperties": {
        "$ref": "#/components/schemas/IJobStatus"
      },
      "type": "object"
    }
  },
  "required": [
    "workerInfo",
    "jobs"
  ],
  "type": "object",
  "additionalProperties": false
}
```

