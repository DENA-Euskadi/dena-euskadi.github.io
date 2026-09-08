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
        "message": {
            "type": "ADMIN_SYNC_METADATA",
            "correlationId": "0777f936-4c31-43b5-81ee-fdf4d708f147",
            "interopRouteData": [
                {
                    "denaComponentId": "DENA_POSTMAN",
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
    "payload": {
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
}
```

| Eremua    | Mota                                           | Derrigorrez | Deskribapena |
|-----------|------------------------------------------------|:-----------:|--------------|
| `context` | [Context](../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, `message.type` `ADMIN_SYNC_METADATA` balioarekin |
| `payload` | [Payload](#payload)                            | ✅          | Eskaeraren payload-a |


## Payload

| Eremua   | Mota     | Derrigorrez | Deskribapena |
|----------|----------|:-----------:|--------------|
| `items`  | `Array`<[SyncMetaDataFromAdminToCOREItem](./modelo/sync-metadata-from-admin-to-core-item.md)>  | ✅ | Pertsona eta administrazioko datu-aldaketen zerrenda |

Elementu bakoitzak `DN00SyncMetaDataFromAdminToCOREItem` modeloa jarraitzen du:

| Eremua                   | Mota                          | Derrigorrez | Deskribapena |
|--------------------------|-------------------------------|:-----------:|--------------|
| `admin`                  | `OrgAdminRef` (`oid`/`id`/`dir3Id`) | ✅ | Aldaketaren jatorrizko administrazioa |
| `aboutPerson`            | `PersonRef` (`oid`/`id`)      | ✅          | Datuak zein pertsonarenak diren |
| `someDataWasUpdatedAt`   | `Instant` (ISO 8601)          | ✅          | Administrazioaren aldean izandako azken aldaketaren unea |
| `ofType`                 | `DataTypeRef` (`oid`/`id`)    | ✅          | Datu mota (marshallTypeId, adib. `administrativeNotice`) |
| `fromDataOriginInstance` | `String`                      | ❌          | Datuaren jatorrizko instantzia (lehenetsia `DEFAULT`) |
| `popMessageAfterSync`    | `PopMessageAfterSyncAdminSpec`| ❌          | Sinkronizazioaren ondoren bezeroan erakusteko mezu aukerakoa |

---

## Erantzun arrakastatsua (HTTP 200)

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "ADMIN_SYNC_METADATA",
            "correlationId": "6750E08A-58F0-433D-900F-253529AAD25E",
            "interopRouteData": [
                {
                    "denaComponentId": "DENA_INTEROP_ADMIN_SYNC",
                    "timestamp": "2026-05-27T15:54:58.8973112Z"
                }
            ]
        }
    },
    "payload": {
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
                        "fromDataOriginInstance": "DEFAULT",
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
