# SyncMetadataFromAdminToCoreItem

## Descripción

Representación de un cambio sobre un dato gestionado por una administración asociado a una persona usuaria de DENA.

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

    click ADMIN "../../../semantica-base/modelo/org-admin-ref/" "Ver OrgAdminRef"
    click PERSON "../../../semantica-base/modelo/person-ref/" "Ver PersonRef"
    click TYPE "../../../semantica-base/modelo/data-type-ref/" "Ver DataTypeRef"
    click MESSAGE "../pop-message-after-sync-spec/" "Ver PopMessageAfterSyncSpec"
```

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (SyncMetadataFromAdminToCoreItem) |
| 🟡 Amarillo | Referencia a otro objeto |
| 🔵 Azul claro | Campos de datos |



## Atributos JSON

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `admin`  | [OrgAdminRef](../../semantica-base/modelo/org-admin-ref.md) | ✅ | Referencia a la administración donde se ha producido el cambio en el dato |
| `aboutPerson` | [PersonRef](../../semantica-base/modelo/person-ref.md) | ✅ | Referencia a la persona asociada al dato que ha sido modificado |
| `someDataWasUpdatedAt` | `Instant` (ISO 8601, **7 decimales**) | ✅ | Instante en que se produjo el cambio. Formato: `yyyy-MM-dd'T'HH:mm:ss.SSSSSSSX` (ej. `2026-11-09T20:41:39.8825952Z`) |
| `ofType` | [DataTypeRef](../../semantica-base/modelo/data-type-ref.md) | ✅ | Referencia al tipo de dato sobre el que se ha producido el cambio |
| `fromDataOriginInstance` | `String` | ❌ | Instancia origen del dato. Usa `DEFAULT` si solo tienes un origen por tipo de dato |
| `popMessageAfterSync` | [PopMessageAfterSyncSpec](./pop-message-after-sync-spec.md) | ❌ | Mensaje a mostrar a la persona usuaria cuando reciba la información del cambio |

## Ejemplo JSON

```json
{
    "admin": {
        "id": "SANDBOXCIF"
    },
    "aboutPerson": {
        "id": "11189396B",
        "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E"
    },
    "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
    "ofType": {
        "id": "PERSON_DATA"
    },
    "fromDataOriginInstance": "DEFAULT"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
