# :material-message-text: Message Types

This page documents the data types used internally in the structure of DENA interoperability messages.

---

## DenaFlowDirection

Enum indicating the direction of the message in the conversation.

| Value | Description |
|---|---|
| `REQ` | Request |
| `RES` | Response to a request |

---

## DenaMessageType

Enum identifying the type of interoperability operation. Each flow has its own message types:

| Flow | Origin | Destination | Value | Description |
|---|---|---|---|---|
| Person-Sync | DENA-CORE | Admin | `DENA_USER_SYNC_PUSH` | Person signup/removal notification |
| Metadata-Sync | Client | DENA-CORE | `UI_META_DATA_SYNC` | Metadata synchronization from the UI |
| Metadata-Sync | Admin | DENA-CORE | `ADMIN_SYNC_PUSH` | Changes sent from the administration (PUSH) |
| Metadata-Sync | DENA-CORE | Admin | `DENA_SYNC_PULL` | Changes request to the administration (PULL) |
| Data-Retrieve | Client | DENA-CORE | `UI_DATA_RETRIEVE` | Data request from the UI |
| Data-Retrieve | DENA-CORE | Admin | `DENA_DATA_RETRIEVE` | Data request to the administration |

---

## DenaInteropRouteDataItem

Every time an interoperability message passes through a DENA component, it **leaves a trace** in the `interopRouteData` array. This information is used primarily for auditing and debugging.

### Attributes

| Field | Type | Description |
|---|---|---|
| `denaComponentId` | `ID` | DENA component identifier |
| `timeStamp` | `TimeStamp` | Instant when the message passed through the component |

### Known component identifiers

| ID | Component |
|---|---|
| `mobileApp` | DENA mobile application |
| `webApp` | DENA web application |
| `apiGateway` | API Gateway |
| `denaCORE` | DENA-CORE (central module) |
| `connector` | Administration connector |

### Example

```json
{
  "interopRouteData": [
    {
      "denaComponentId": "mobileApp",
      "timeStamp": 1670374400
    },
    {
      "denaComponentId": "denaCORE",
      "timeStamp": 1670374401
    },
    {
      "denaComponentId": "connector",
      "timeStamp": 1670374402
    }
  ]
}
```

---

## DenaPersonAndConsentGiven

Structure combining minimal person data with granted consent data.

| Field | Type | Description |
|---|---|---|
| `personRef` | [DenaPersonRef](./person-ref.md) | Minimal person data (oid, id, signup date, etc.) |
| `consentRef` | `DenaConsentRef` | Consent reference (oid, url, etc.) |

### Example

```json
{
  "personRef": {
    "personId": "12345678A",
    "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2"
  },
  "consentRef": {
    "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentURL": "https://interop.api.dena.eus/consent/db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
  }
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
