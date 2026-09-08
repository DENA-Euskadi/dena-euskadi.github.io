# :material-message-text: Message Types

This page documents the types used in the structure of the DENA **interop message** (`DN00InteropContext` and `DN00InteropMessageData`).

---

## DN00InteropFlowDirection

Enum indicating the direction of the message. It is not stored independently: it is derived from the message type.

| Value | Description |
|---|---|
| `REQUEST` | Request |
| `RESPONSE` | Response to a request |

---

## DN00InteropMessageType

Enum identifying the type of interoperability operation. These are the actual values of the enum:

| Value | Scope |
|---|---|
| `CLIENT_LOGIN`, `CLIENT_LOGIN_DEMO` | Client login |
| `CLIENT_PASSKEY_REGISTER_INIT` / `CLIENT_PASSKEY_REGISTER_FINISH` | Passkey registration |
| `CLIENT_PASSKEY_LOGIN_INIT` / `CLIENT_PASSKEY_LOGIN_FINISH` | Passkey login |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS` | Passkey credentials cleanup |
| `CLIENT_INIT_REQ` / `CLIENT_INIT_RESP` | Client initialization |
| `CLIENT_SRMD_SYNC_REQ` / `CLIENT_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) from the client |
| `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) from the administration |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Data-Retrieve from the client |
| `PERSON_FETCH_DATA` | Retrieval of a person's data |
| `ADMIN_PERSON_PULL_BESPOKE_CREATE_REQ` | Person-Sync Pull: create bespoke job |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH` | Person-Sync Pull: query bespoke job |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` | Bespoke asset download |
| `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH` | Pregenerated asset download |
| `PERSON_ADMIN_SEARCH` | Person search by the administration |
| `PERSON_ADMIN_HEAD` | Person HEAD query by the administration |

---

## DN00IteropRouteDataItem

Every time a message passes through a DENA component, it **leaves a trace** in the `interopRouteData` array. It is used for auditing and debugging.

### Attributes

| Field | Type | Description |
|---|---|---|
| `denaComponentId` | `DN00InteropComponent` | DENA component identifier |
| `timestamp` | `Instant` (ISO 8601) | Instant when the message passed through the component |

### Component identifiers

| Value | Component |
|---|---|
| `CLIENT_INSTALLMENT` | Client installation (DENA-APP) |
| `DENA_CORE` | DENA-CORE (central module) |
| `DENA_ADMIN_CONNECTOR` | Administration connector |
| `ADMIN` | Administration system |

### Example

```json
{
  "interopRouteData": [
    {
      "denaComponentId": "CLIENT_INSTALLMENT",
      "timestamp": "2026-08-18T11:28:47.523Z"
    },
    {
      "denaComponentId": "DENA_CORE",
      "timestamp": "2026-08-18T11:28:47.601Z"
    },
    {
      "denaComponentId": "DENA_ADMIN_CONNECTOR",
      "timestamp": "2026-08-18T11:28:47.688Z"
    }
  ]
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
