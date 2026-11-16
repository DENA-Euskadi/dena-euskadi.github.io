# SyncMetadataFromAdminToCoreItem

## Deskribapena

DENAren erabiltzaile bati lotutako administrazio batek kudeatutako datu batean gertatutako aldaketaren irudikapena.

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

    click ADMIN "../../../semantica-base/modelo/org-admin-ref/" "Ikusi OrgAdminRef"
    click PERSON "../../../semantica-base/modelo/person-ref/" "Ikusi PersonRef"
    click TYPE "../../../semantica-base/modelo/data-type-ref/" "Ikusi DataTypeRef"
    click MESSAGE "../pop-message-after-sync-spec/" "Ikusi PopMessageAfterSyncSpec"
```

| Kolorea | Esanahia |
|---------|----------|
| 🟠 Laranja | Objektu nagusia (SyncMetadataFromAdminToCoreItem) |
| 🟡 Hori | Beste objektu baten erreferentzia |
| 🔵 Urdin argia | Datu-eremuak |



## JSON atributuak

| Eremua   | Mota     | Derrigorrez | Deskribapena |
|----------|----------|:-----------:|--------------|
| `admin`  | [OrgAdminRef](../../semantica-base/modelo/org-admin-ref.md) | ✅ | Datu-aldaketa gertatu den administrazioaren erreferentzia |
| `aboutPerson` | [PersonRef](../../semantica-base/modelo/person-ref.md) | ✅ | Aldatutako datuari lotutako pertsonaren erreferentzia |
| `someDataWasUpdatedAt` | `Instant` (ISO 8601, **7 dezimal**) | ✅ | Aldaketa gertatu zen unea. Formatua: `yyyy-MM-dd'T'HH:mm:ss.SSSSSSSX` (adib. `2026-11-09T20:41:39.8825952Z`) |
| `ofType` | [DataTypeRef](../../semantica-base/modelo/data-type-ref.md) | ✅ | Aldaketa gertatu den datu motaren erreferentzia |
| `fromDataOriginInstance` | `String` | ❌ | Datu-jatorriaren instantzia. Erabili `DEFAULT` datu-mota bakoitzeko jatorri bakarra baduzu |
| `popMessageAfterSync` | [PopMessageAfterSyncSpec](./pop-message-after-sync-spec.md) | ❌ | Erabiltzaileari aldaketaren informazioa jasotzen duenean erakusteko mezua |

## JSON adibidea

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
