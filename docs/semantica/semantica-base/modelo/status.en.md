# :material-check-circle: Status (Response)

## Description

The `status` structure is present **only in response messages**. It includes information about the processing result: whether it was successful, there was an error, or it will be processed asynchronously.

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `statusCode` | `DENAResponseStatusCode` | :material-check: | Message processing status code |
| `statusDetails` | `Object` | :material-close: | Response details. Type depends on `statusCode` |

---

## DENAResponseStatusCode

| Value | Description |
|---|---|
| `OK` | Message processed successfully |
| `CLIENT_ERR` | Client error (e.g. invalid data) |
| `SERVER_ERR` | Server error |
| `QUEUED` | Message queued for asynchronous processing |

---

## Status Details by code

| statusCode | statusDetails type |
|---|---|
| `OK` | *(empty)* |
| `CLIENT_ERR` | [DENAClientErrDetails](#denaclienterrdetails) |
| `SERVER_ERR` | [DENAServerErrDetails](#denaservererrdetails) |
| `QUEUED` | [DENAAsyncQueueData](#denaasyncqueuedata) |

---

## DENAClientErrDetails

Error details when `statusCode = CLIENT_ERR`.

| Field | Type | Description |
|---|---|---|
| `errorCode` | `ID` | Error identifier. E.g. `ENTITY_NOT_FOUND` |
| `about` | `String` | Data about the entity related to the error (e.g. personOid/personId) |
| `errorDetails` | `String` | Additional error details |

**Example:**

```json
{
  "status": {
    "statusCode": "CLIENT_ERR",
    "statusDetails": {
      "errorCode": "ENTITY_NOT_FOUND",
      "about": "12345678A",
      "errorDetails": "The requested person does not exist in the system"
    }
  }
}
```

---

## DENAServerErrDetails

Error details when `statusCode = SERVER_ERR`.

| Field | Type | Description |
|---|---|---|
| `errorCode` | `ID` | Error identifier. E.g. `UNEXPECTED_ERROR` |
| `errorDetails` | `String` | Error details (e.g. stack trace) |

**Example:**

```json
{
  "status": {
    "statusCode": "SERVER_ERR",
    "statusDetails": {
      "errorCode": "UNEXPECTED_ERROR",
      "errorDetails": "Connection timeout accessing database"
    }
  }
}
```

---

## DENAAsyncQueueData

Details when `statusCode = QUEUED` (asynchronous processing).

| Field | Type | Description |
|---|---|---|
| `jobToken` | `Token` | Token assigned to the scheduled job, allows querying its status |
| `jobStatusQueryUrl` | `URL` | URL to query the job status |

**Example:**

```json
{
  "status": {
    "statusCode": "QUEUED",
    "statusDetails": {
      "jobToken": "a3f2c891-4b5d-4e6f-8a9b-1c2d3e4f5a6b",
      "jobStatusQueryUrl": "https://interop.api.dena.eus/queued-jobs/a3f2c891-4b5d-4e6f-8a9b-1c2d3e4f5a6b"
    }
  }
}
```

---

## Querying an asynchronous Job status

When querying `jobStatusQueryUrl`, the response contains:

| Field | Type | Description |
|---|---|---|
| `jobToken` | `Token` | Token of the queried job |
| `jobStatus` | `Enum` | Current job status |

**`jobStatus` values:**

| Value | Description |
|---|---|
| `QUEUED` | Queued, pending execution |
| `EXECUTING` | Currently executing |
| `FINISHED` | Completed successfully |
| `FAILED` | Completed with error |
| `DISCARDED` | Discarded |

---

## Asynchronous processing

!!! info "Asynchronous flow"
    In asynchronous invocations, the result is NOT included in the immediate response message. A token is returned and the message is queued. When processing completes, there are two options:

    1. **Callback**: the server notifies the origin at the callback URL provided in the `protocol` section
    2. **Polling**: the origin periodically queries the status using `jobStatusQueryUrl`

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
