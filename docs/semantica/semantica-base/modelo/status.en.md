# :material-check-circle: Status (Response)

## Description

Response messages (`DN00InteropResponseMessageBase`) include information about the processing result through three fields: `code`, `errorId` and `details`.

---

## JSON attributes

| Field | Type | Required | Description |
|---|---|:---:|---|
| `code` | `DN00InteropResponseStatus` | :material-check: | Status code of the message processing |
| `errorId` | `DN00InteropResponseStatusCode` | :material-close: | Specific error code (present only on errors) |
| `details` | `DN00InteropResponseStatusDetails` | :material-close: | Response details (a single `details` text field) |

---

## DN00InteropResponseStatus (`code` field)

| Value | Description |
|---|---|
| `OK` | Message processed successfully |
| `CLIENT_ERR` | Client error (e.g. invalid data) |
| `SERVER_ERR` | Server error |
| `QUEUED` | The message has been placed in the asynchronous processing queue |

---

## Examples

**Successful response:**

```json
{
  "code": "OK",
  "errorId": null,
  "details": null
}
```

**Response with client error:**

```json
{
  "code": "CLIENT_ERR",
  "errorId": "ENTITY_NOT_FOUND",
  "details": {
    "details": "La persona solicitada no existe en el sistema"
  }
}
```

**Response with server error:**

```json
{
  "code": "SERVER_ERR",
  "errorId": "UNEXPECTED_ERROR",
  "details": {
    "details": "Connection timeout accessing database"
  }
}
```

!!! note "`details` structure"
    `DN00InteropResponseStatusDetails` is a class with a single text field (`details`). There are no specific subtypes per error code in the current model: the error detail is transmitted as free text.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
