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
        "admin": { "id": "SANDBOXCIF" },
        "aboutPerson": { "id": "11189396B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
        "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
        "ofType": { "id": "PERSON_DATA" }
    },
    {
        "admin": { "id": "SANDBOXCIF" },
        "aboutPerson": { "id": "11111111B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
        "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
        "ofType": { "id": "PERSON_DATA" }
    }
]
```

!!! info "Usage"
    DENA-CORE compares `someDataWasUpdatedAt` with the last time that data type was retrieved from the source to decide the NEW/UPDATED/UNCHANGED state shown in the UI.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
