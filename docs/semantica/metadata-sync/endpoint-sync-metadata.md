# Endpoint SYNC-METADATA

## Endpoint

```
POST https://api-batera.pre.dena.eus/sync/api/admin/interop/sync/metadata
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
            "correlationId": "9cd46653-284c-468d-81f6-53c84bd50794",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-11-10T08:07:38.934Z"
                }
            ]
        },
        "originAdmin": {
            "id": "SANDBOXCIF"
        }
    },
    "payload": [
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
            }
        },
        {
            "admin": {
                "id": "SANDBOXCIF"
            },
            "aboutPerson": {
                "id": "11111111B",
                "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E"
            },
            "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
            "ofType": {
                "id": "PERSON_DATA"
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
| `ofType`                 | `DataTypeRef` (`oid`/`id`)    | ✅          | Tipo de dato (ID externo, ej. `ADMINISTRATIVE_NOTICE`). Ver [catálogo](../semantica-base/modelo/data-type-ref.md) |
| `fromDataOriginInstance` | `String`                      | ❌          | Instancia origen del dato (por defecto `DEFAULT`) |
| `popMessageAfterSync`    | `PopMessageAfterSyncAdminSpec`| ❌          | Mensaje opcional a mostrar en cliente tras la sincronización |

---

## Response exitosa (HTTP 200)

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "ADMIN_SRMD_SYNC_RESP",
            "correlationId": "9cd46653-284c-468d-81f6-53c84bd50794",
            "interopRouteData": [
                {
                    "denaComponentId": "DENA_INTEROP_ADMIN_SYNC",
                    "timestamp": "2026-11-10T08:07:39.5179823Z"
                },
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-11-10T08:07:38.934Z"
                }
            ]
        }
    },
    "payload": {
        "transactionOid": "FFCD3371-D351-427E-A890-0C886BCA1275",
        "receivedItemsCount": 2,
        "processedOK": [
            {
                "admin": {
                    "id": "SANDBOXCIF",
                    "oid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011"
                },
                "dataLastUpdatedAt": "2026-11-09T20:41:39.8825952Z",
                "dataOriginInstanceId": "DEFAULT",
                "dataType": {
                    "id": "PERSON_DATA",
                    "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4007"
                },
                "entityVersion": 5,
                "lastTransactionOid": "FFCD3371-D351-427E-A890-0C886BCA1275",
                "numericId": 0,
                "oid": "6485BF2C-5C53-4EC0-ABE3-212DD4403913",
                "person": {
                    "id": "11189396B",
                    "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E"
                },
                "trackingInfo": {
                    "createdAt": "2026-11-04T17:22:41.6877234Z",
                    "lastUpdatedAt": "2026-11-10T08:07:39.4321202Z",
                    "lastUpdaterSecurityPrincipalOid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011"
                }
            }
        ],
        "processedNOK": [
            {
                "item": {
                    "admin": {
                        "id": "SANDBOXCIF",
                        "oid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011"
                    },
                    "aboutPerson": {
                        "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E"
                    },
                    "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
                    "ofType": {
                        "id": "PERSON_DATA",
                        "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4007"
                    },
                    "fromDataOriginInstance": "DEFAULT"
                },
                "error": "The [person] with ref=631011F3-9A2F-4D08-9F56-B7F4D6A0749E;null could NOT be validated"
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
