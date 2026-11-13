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

Enum identifying the type of interoperability operation. Complete list of `DN00InteropMessageType` enum values with their direction and involved parties (source: `DN00InteropMessageType.java`):

| Request (`_REQ`) / Response (`_RESP`) | Operation | Origin → Destination |
|---|---|---|
| `CLIENT_LOGIN_DEMO` | Client demo login (no paired response) | CLIENT_INSTALLMENT → DENA_CORE |
| `CLIENT_LOGIN_REQ` / `CLIENT_LOGIN_RESP` | Client login | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_REFRESH_TOKEN_REQ` / `CLIENT_REFRESH_TOKEN_RESP` | Token refresh | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS_REQ` / `CLIENT_PASSKEY_CLEAN_CREDENTIALS_RESP` | Passkey credentials cleanup | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_LOGIN_INIT_REQ` / `CLIENT_PASSKEY_LOGIN_INIT_RESP` | Passkey login — init | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_LOGIN_FINISH_REQ` / `CLIENT_PASSKEY_LOGIN_FINISH_RESP` | Passkey login — finish | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_REGISTER_INIT_REQ` / `CLIENT_PASSKEY_REGISTER_INIT_RESP` | Passkey registration — init | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_REGISTER_FINISH_REQ` / `CLIENT_PASSKEY_REGISTER_FINISH_RESP` | Passkey registration — finish | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_LOGIN_REQ` / `ADMIN_LOGIN_RESP` | Administration login | ADMIN ↔ DENA_CORE |
| `CLIENT_INIT_REQ` / `CLIENT_INIT_RESP` | Client initialization | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_SRMD_SYNC_REQ` / `CLIENT_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) initiated from the client | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) initiated from the administration | ADMIN ↔ DENA_CORE |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Data-Retrieve: data retrieval from the client | CLIENT_INSTALLMENT ↔ ADMIN |
| `CLIENT_USER_DELETE_REQ` / `CLIENT_USER_DELETE_RESP` | User deletion from the client | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_USER_DELETE_BY_ID_REQ` / `CLIENT_USER_DELETE_BY_ID_RESP` | User deletion by ID from the client | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `PERSON_ADMIN_SEARCH_REQ` / `PERSON_ADMIN_SEARCH_RESP` | Person search by the administration | ADMIN ↔ DENA_CORE |
| `PERSON_FETCH_DATA_REQ` / `PERSON_FETCH_DATA_RESP` | Retrieval of a person's data | ADMIN ↔ DENA_CORE |
| `PERSON_ADMIN_HEAD_REQ` / `PERSON_ADMIN_HEAD_RESP` | Person HEAD query by the administration | ADMIN ↔ DENA_CORE |
| `PERSON_PUSH_TO_ADMIN_REQ` / `PERSON_PUSH_TO_ADMIN_RESP` | Person-Sync Push: DENA sends person data to administration | DENA_CORE ↔ ADMIN |
| `CLIENT_PERSON_FETCH_REQ` / `CLIENT_PERSON_FETCH_RESP` | Person data query from client device | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PERSON_UPDATE_REQ` / `CLIENT_PERSON_UPDATE_RESP` | Person data update from client device | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_BESPOKE_CREATE_REQ` / `ADMIN_PERSON_PULL_BESPOKE_CREATE_RESP` | Person-Sync Pull: create bespoke job | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH_REQ` / `ADMIN_PERSON_PULL_BESPOKE_FETCH_RESP` | Person-Sync Pull: query bespoke job result | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_PREGEN_FETCH_REQ` / `ADMIN_PERSON_PULL_PREGEN_FETCH_RESP` | Person-Sync Pull: query pregenerated job result | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_RESP` | Person-Sync Pull: bespoke asset download | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_RESP` | Person-Sync Pull: pregenerated asset download | ADMIN ↔ DENA_CORE |

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
| `DENA_INTEROP_ADMIN_SYNC` | Administration interop synchronisation component |
| `ADMIN` | Administration system |
| `DENA_PERSON_ADMIN` | DENA Person Administration module |
| `DENA_PERSON_ADMIN_SYNC` | DENA Person Synchronisation Service |
| `DENA_SANDBOX_ADMIN` | DENA Sandbox Administration (test environments) |

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
