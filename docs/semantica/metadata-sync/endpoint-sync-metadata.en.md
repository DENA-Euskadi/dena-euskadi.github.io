# Endpoint SYNC-METADATA

## Endpoint

```
POST /api/admin/interop/sync/metadata
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

---

## Request

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

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../semantica-base/index.md)          | ✅        | Request context object, with `message.type` set to `ADMIN_SYNC_METADATA` |
| `payload` | [Payload](#payload)                            | ✅        | Request payload |


## Payload

| Field    | Type     | Mandatory | Description |
|----------|----------|:---------:|-------------|
| `items`  | `Array`<[SyncMetaDataFromAdminToCOREItem](./modelo/sync-metadata-from-admin-to-core-item.md)>  | ✅ | List of data changes per person and administration |

Each item follows the `DN00SyncMetaDataFromAdminToCOREItem` model:

| Field                    | Type                          | Mandatory | Description |
|--------------------------|-------------------------------|:---------:|-------------|
| `admin`                  | `OrgAdminRef` (`oid`/`id`/`dir3Id`) | ✅ | Origin administration of the change |
| `aboutPerson`            | `PersonRef` (`oid`/`id`)      | ✅        | Person the data belongs to |
| `someDataWasUpdatedAt`   | `Instant` (ISO 8601)          | ✅        | Timestamp of the last change on the admin side |
| `ofType`                 | `DataTypeRef` (`oid`/`id`)    | ✅        | Data type (marshallTypeId, e.g. `administrativeNotice`) |
| `fromDataOriginInstance` | `String`                      | ❌        | Data origin instance (defaults to `DEFAULT`) |
| `popMessageAfterSync`    | `PopMessageAfterSyncAdminSpec`| ❌        | Optional message to show on the client after syncing |

---

## Successful response (HTTP 200)

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

## Error response (HTTP 4xx/5xx)

```
Error: The items to be synced cannot be null or empty
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
