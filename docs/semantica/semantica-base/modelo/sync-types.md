# :material-sync: Tipos de Datos del Flujo Sync

Esta página documenta la estructura utilizada en el **flujo de sincronización de metadatos** (Metadata-Sync / SRMD) que una administración envía a DENA-CORE.

---

## DN00SyncMetaDataFromAdminToCOREItem

Ítem que una administración envía a DENA-CORE para notificar que **algún dato de una persona se actualizó** en el origen, para un tipo de dato concreto. Se envía como un array de estos ítems.

Clase: `DN00SyncMetaDataFromAdminToCOREItem` (`@MarshallType(as="syncMetaDataFromAdminToCORE")`).

### Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `admin` | [OrgAdminRef](./org-admin-ref.md) | :material-check: | Administración de origen |
| `aboutPerson` | [PersonRef](./person-ref.md) | :material-check: | Persona a la que pertenece el dato |
| `someDataWasUpdatedAt` | `Instant` | :material-check: | Última vez que el dato se actualizó en la administración |
| `ofType` | [DataTypeRef](./data-type-ref.md) | :material-check: | Tipo de dato |
| `fromDataOriginInstance` | `ID` | :material-close: | Instancia de origen de datos (qué sistema de gestión proporciona el dato) |
| `popMessageAfterSync` | `Object` | :material-close: | Mensaje opcional a mostrar en el cliente tras sincronizar (p. ej. "Tienes un nuevo expediente en XX") |

### Ejemplo

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

!!! info "Uso"
    DENA-CORE compara `someDataWasUpdatedAt` con la última vez que ese tipo de dato fue recuperado del origen para decidir el estado NEW/UPDATED/UNCHANGED que se muestra en la UI.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
