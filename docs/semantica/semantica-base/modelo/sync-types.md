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

!!! info "Uso"
    DENA-CORE compara `someDataWasUpdatedAt` con la última vez que ese tipo de dato fue recuperado del origen para decidir el estado NEW/UPDATED/UNCHANGED que se muestra en la UI.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
