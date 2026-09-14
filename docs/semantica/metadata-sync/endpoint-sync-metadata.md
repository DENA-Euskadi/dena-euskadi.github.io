# Endpoint SYNC-METADATA

## Endpoint

```
POST /api/admin/interop/sync/metadata
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

---

## Request

```json
{
    "context": {
        "message": {
            "type": "ADMIN_SRMD_SYNC_REQ",
            "correlationId": "0777f936-4c31-43b5-81ee-fdf4d708f147",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-06-10T15:37:57.5530000Z"
                }
            ]
        },
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originAdmin": {
            "id": "admin-A414",
            "dir3Id": "EA0000001"
        }
    },
    "payload": [
        {
            "admin": {
                "id": "admin-A414"
            },
            "aboutPerson": {
                "id": "12345678A"
            },
            "someDataWasUpdatedAt": "2026-05-11T09:56:10.2237636Z",
            "ofType": {
                "id": "administrativeNotice"
            },
            "fromDataOriginInstance": "DEFAULT",
            "popMessageAfterSync": {
                "how": "AT_CLIENT_AFTER_SYNC",
                "messageByLang": {
                    "SPANISH": "Nuevo expediente de \"adminId\"",
                    "ENGLISH": "New procedure from \"adminId\""
                }
            }
        }
    ]
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md)          | ✅          | Objeto de contexto de la petición, con `message.type` con valor `ADMIN_SRMD_SYNC_REQ` |
| `payload` | `Array`<[SyncMetaDataFromAdminToCOREItem](./modelo/sync-metadata-from-admin-to-core-item.md)> | ✅ | **Array** con los cambios en datos por persona y administración. El array va directamente bajo `payload` (no hay envoltorio `items`) |


## Payload (elementos del array)

Cada elemento del array `payload` sigue el modelo `DN00SyncMetaDataFromAdminToCOREItem`:

| Campo                    | Tipo                          | Obligatorio | Descripción |
|--------------------------|-------------------------------|-------------|-------------|
| `admin`                  | `OrgAdminRef` (`oid`/`id`/`dir3Id`) | ✅ | Administración origen del cambio |
| `aboutPerson`            | `PersonRef` (`oid`/`id`)      | ✅          | Persona a la que pertenecen los datos |
| `someDataWasUpdatedAt`   | `Instant` (ISO 8601)          | ✅          | Instante del último cambio en el lado de la admin |
| `ofType`                 | `DataTypeRef` (`oid`/`id`)    | ✅          | Tipo de dato (marshallTypeId, ej. `administrativeNotice`) |
| `fromDataOriginInstance` | `String`                      | ❌          | Instancia origen del dato (por defecto `DEFAULT`) |
| `popMessageAfterSync`    | `PopMessageAfterSyncAdminSpec`| ❌          | Mensaje opcional a mostrar en cliente tras la sincronización |

---

## Response exitosa (HTTP 200)

```json
{
    "context": {
        "message": {
            "type": "ADMIN_SRMD_SYNC_RESP",
            "correlationId": "6750E08A-58F0-433D-900F-253529AAD25E",
            "interopRouteData": [
                {
                    "denaComponentId": "DENA_CORE",
                    "timestamp": "2026-05-27T15:54:58.8973112Z"
                }
            ]
        }
    },
    "code": "OK",
    "payload": {
        "transactionOid": "EA2C9ECD-DC48-48D0-B863-963D9468F042",
        "receivedItemsCount": 1,
        "processedOK": [],
        "processedNOK": [
            {
                "item": {
                    "admin": { "id": "admin-A414" },
                    "aboutPerson": { "id": "12345678A" },
                    "someDataWasUpdatedAt": "2026-05-11T09:56:10.2237636Z",
                    "ofType": { "id": "administrativeNotice" },
                    "fromDataOriginInstance": "DEFAULT"
                },
                "error": "The admin with ref=null;admin-A414 could NOT be validated"
            }
        ]
    }
}
```

> El `payload` de la respuesta es directamente el objeto `DN00SyncMetaDataFromAdminCOREProcessingInfo` (`@MarshallType(as="syncMetaDataFromAdminCOREProcessingInfo")`); **no hay envoltorio `processingInfo`**. El `code` (estado del procesamiento, valores `OK`/`CLIENT_ERR`/`SERVER_ERR`/`QUEUED`) va a nivel raíz, hermano de `context` y `payload`.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `transactionOid` | `String` | OID de transacción asignado por CORE para esta sincronización |
| `receivedItemsCount` | `Number` | Número de ítems recibidos |
| `processedOK` | `Array` | Ítems procesados correctamente (SRMD) |
| `processedNOK` | `Array` | Ítems que fallaron, cada uno con `item` (el ítem original) y `error` (motivo) |

## Response de error (HTTP 4xx/5xx)

```
Error: The items to be synced cannot be null or empty
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
