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
        <Objeto de Contexto>
    },
    "data": {
        "ofType": "syncMetaDataFromAdminRequestPayload",
        "items": [
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
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md) | ✅          | Objeto de contexto de la petición |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |


## Data

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `ofType` | `String` | ✅ | `"syncMetaDataFromAdminRequestPayload"` |
| `items`  | `Array`<[SyncMetaDataFromAdminToCOREItem](./modelo/sync-metadata-from-admin-to-core-item.md)>  | ✅ | Listado con los cambios en datos por persona y administración |

---

## Response exitosa (HTTP 200)

```json
{
    "code": "OK",
    "context": {
        "flowDirection": "RESPONSE",
        "interopRouteData": [
            {
                "denaComponentId": "DENA_INTEROP_ADMIN_SYNC",
                "timestamp": "2026-05-27T15:54:58.8973112Z"
            }
        ],
        "messageCorrelationId": "6750E08A-58F0-433D-900F-253529AAD25E"
    },
    "data": {
        "processingInfo": {
            "transactionOid": "EA2C9ECD-DC48-48D0-B863-963D9468F042",
            "receivedItemsCount": 1,
            "processedNOK": [
                {
                    "item": {
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
                        "popMessageAfterSync": {
                            "how": "AT_CLIENT_AFTER_SYNC",
                            "messageByLang": {
                                "SPANISH": "Nuevo expediente de \"adminId\"",
                                "ENGLISH": "New procedure from \"adminId\""
                            }
                        }
                    },
                    "error": "The admin with ref=null;{{adminId}} could NOT be validated"
                }
            ]
        }
    }
}
```

## Response de error (HTTP 4xx/5xx)

```json

```
