# :material-sync: Sync Flow Data Types

This page documents the structure used in the **metadata synchronization flow** (Metadata-Sync / SRMD) that an administration sends to DENA-CORE.

---

## DN00SyncMetaDataFromAdminToCOREItem

Item that an administration sends to DENA-CORE to notify that **some data of a person was updated** at the source, for a specific data type. It is sent as an array of these items.

Class: `DN00SyncMetaDataFromAdminToCOREItem` (`@MarshallType(as="syncMetaDataFromAdminToCORE")`).

### JSON attributes

| Field | Type | Required | Description |
|---|---|:---:|---|
| `admin` | [OrgAdminRef](./org-admin-ref.md) | :material-check: | Source administration |
| `aboutPerson` | [PersonRef](./person-ref.md) | :material-check: | Person the data belongs to |
| `someDataWasUpdatedAt` | `Instant` | :material-check: | Last time the data was updated at the administration |
| `ofType` | [DataTypeRef](./data-type-ref.md) | :material-check: | Data type |
| `fromDataOriginInstance` | `ID` | :material-close: | Data origin instance (which management system provides the data) |
| `popMessageAfterSync` | `Object` | :material-close: | Optional message to show on the client after synchronizing (e.g. "You have a new record at XX") |

### Example

```json
[
  {
    "admin": { "id": "EJGV", "oid": "..." },
    "aboutPerson": { "id": "11111111H", "oid": "..." },
    "someDataWasUpdatedAt": "2025-07-16T13:00:00.000Z",
    "ofType": { "id": "administrativeServiceProcedureRecord", "oid": "..." },
    "fromDataOriginInstance": "EJGV-Expedientes"
  },
  {
    "admin": { "id": "DFB", "oid": "..." },
    "aboutPerson": { "id": "11223344L", "oid": "..." },
    "someDataWasUpdatedAt": "2025-07-15T11:00:00.000Z",
    "ofType": { "id": "oneOffPayment", "oid": "..." },
    "fromDataOriginInstance": "DFB-Pagos"
  }
]
```

!!! info "Usage"
    DENA-CORE compares `someDataWasUpdatedAt` with the last time that data type was retrieved from the source to decide the NEW/UPDATED/UNCHANGED state shown in the UI.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
