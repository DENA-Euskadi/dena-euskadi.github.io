# Endpoint SYNC-METADATA

## Endpoint

```
POST /api/admin/interop/sync/metadata
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

---

## Eskaera

```json
{
    "context": {
        "interopRouteData": [
            {
                "denaComponentId": "DENA_POSTMAN",
                "timestamp":"2026-06-10T15:37:57.5530000Z"
            }
        ],
        "messageCorrelationId": "0777f936-4c31-43b5-81ee-fdf4d708f147",
        "messageType": "ADMIN_SYNC_METADATA",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originPartyId": "ADMIN-001",
        "destinationPartyId": "DENA_INTEROP"
    },
    "data": {
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

| Eremua    | Mota                                           | Derrigorrez | Deskribapena |
|-----------|------------------------------------------------|:-----------:|--------------|
| `context` | [Context](../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, `ADMIN_SYNC_METADATA` balioarekin messageType barne |
| `data`    | [Data](#data)                                  | ✅          | Eskaeraren payload-a |


## Data

| Eremua   | Mota     | Derrigorrez | Deskribapena |
|----------|----------|:-----------:|--------------|
| `items`  | `Array`<[SyncMetaDataFromAdminToCOREItem](./modelo/sync-metadata-from-admin-to-core-item.md)>  | ✅ | Pertsona eta administrazioko datu-aldaketen zerrenda |

---

## Erantzun arrakastatsua (HTTP 200)

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
                    "error": "The admin with ref=null;admin-A414 could NOT be validated"
                }
            ]
        }
    }
}
```

## Errore-erantzuna (HTTP 4xx/5xx)

```
Error: The items to be synced cannot be null or empty
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
