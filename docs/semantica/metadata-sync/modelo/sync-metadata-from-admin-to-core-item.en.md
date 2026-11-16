# SyncMetadataFromAdminToCoreItem

## Description

Representation of a change to a piece of data managed by an administration, associated with a DENA user.

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#000000"
    primaryBorderColor: "#CCCCCC"
    lineColor: "#4A4A4A"
    fontSize: "12px"
---
flowchart LR
    SYNC["<b>SyncMetadataFromAdminToCoreItem</b>"]

    SYNC --> ADMIN["admin<br/><i>OrgAdminRef</i>"]
    SYNC --> PERSON["aboutPerson<br/><i>PersonRef</i>"]
    SYNC --> DATE["someDataWasUpdatedAt<br/><i>ISO 8601 Date</i>"]
    SYNC --> TYPE["ofType<br/><i>DataTypeRef</i>"]
    SYNC --> ORIGIN["fromDataOriginInstance<br/><i>String</i>"]
    SYNC --> MESSAGE["popMessageAfterSync<br/><i>PopMessageAfterSyncSpec</i>"]

    style SYNC fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style ADMIN fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style PERSON fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style DATE fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style TYPE fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style MESSAGE fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style ORIGIN fill:#e8f5e8,stroke:#2e7d32,color:#000000,rx:6,ry:6

    click ADMIN "../../../semantica-base/modelo/org-admin-ref/" "View OrgAdminRef"
    click PERSON "../../../semantica-base/modelo/person-ref/" "View PersonRef"
    click TYPE "../../../semantica-base/modelo/data-type-ref/" "View DataTypeRef"
    click MESSAGE "../pop-message-after-sync-spec/" "View PopMessageAfterSyncSpec"
```

| Colour | Meaning |
|--------|---------|
| 🟠 Orange | Main object (SyncMetadataFromAdminToCoreItem) |
| 🟡 Yellow | Reference to another object |
| 🔵 Light blue | Data fields |



## JSON attributes

| Field    | Type     | Mandatory | Description |
|----------|----------|:---------:|-------------|
| `admin`  | [OrgAdminRef](../../semantica-base/modelo/org-admin-ref.md) | ✅ | Reference to the administration where the data change occurred |
| `aboutPerson` | [PersonRef](../../semantica-base/modelo/person-ref.md) | ✅ | Reference to the person associated with the modified data |
| `someDataWasUpdatedAt` | `Instant` (ISO 8601, **7 decimal places**) | ✅ | Instant when the change occurred. Format: `yyyy-MM-dd'T'HH:mm:ss.SSSSSSSX` (e.g. `2026-11-09T20:41:39.8825952Z`) |
| `ofType` | [DataTypeRef](../../semantica-base/modelo/data-type-ref.md) | ✅ | Reference to the data type on which the change occurred |
| `fromDataOriginInstance` | `String` | ❌ | Data origin instance identifier. Use `DEFAULT` if you have a single origin per data type |
| `popMessageAfterSync` | [PopMessageAfterSyncSpec](./pop-message-after-sync-spec.md) | ❌ | Message to display to the user when they receive the change information |

## JSON example

```json
{
    "admin": {
        "id": "admin-A414"
    },
    "aboutPerson": {
        "id": "12345678A"
    },
    "someDataWasUpdatedAt": "2026-05-11T09:56:10.2237636Z",
    "ofType": {
        "id": "ADMINISTRATIVE_NOTICE"
    },
    "popMessageAfterSync": {
        "how": "AT_CLIENT_AFTER_SYNC",
        "messageByLang": {
            "SPANISH": "Nuevo expediente de \"adminId\"",
            "ENGLISH": "New procedure from \"adminId\""
        }
    }
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
