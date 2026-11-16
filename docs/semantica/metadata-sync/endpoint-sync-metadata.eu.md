# Endpoint SYNC-METADATA

## Endpoint

```
POST https://api-batera.pre.dena.eus/sync/api/admin/interop/sync/metadata
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

| Eremua    | Mota                                           | Derrigorrez | Deskribapena |
|-----------|------------------------------------------------|:-----------:|--------------|
| `context` | [Context](../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, `message.type` `ADMIN_SRMD_SYNC_REQ` balioarekin |
| `payload` | `Array`<[SyncMetaDataFromAdminToCOREItem](./modelo/sync-metadata-from-admin-to-core-item.md)> | ✅ | Pertsona eta administrazioko datu-aldaketen **array**-a. Array-a zuzenean `payload`-en azpian doa (ez dago `items` bilgarririk) |


## Payload (array-aren elementuak)

`payload` array-aren elementu bakoitzak `DN00SyncMetaDataFromAdminToCOREItem` modeloa jarraitzen du:

| Eremua                   | Mota                          | Derrigorrez | Deskribapena |
|--------------------------|-------------------------------|:-----------:|--------------|
| `admin`                  | `OrgAdminRef` (`oid`/`id`/`dir3Id`) | ✅ | Aldaketaren jatorrizko administrazioa |
| `aboutPerson`            | `PersonRef` (`oid`/`id`)      | ✅          | Datuak zein pertsonarenak diren |
| `someDataWasUpdatedAt`   | `Instant` (ISO 8601)          | ✅          | Administrazioaren aldean izandako azken aldaketaren unea |
| `ofType`                 | `DataTypeRef` (`oid`/`id`)    | ✅          | Datu mota (ID kanpokoa, adib. `ADMINISTRATIVE_NOTICE`). Ikusi [katalogoa](../semantica-base/modelo/data-type-ref.md) |
| `fromDataOriginInstance` | `String`                      | ❌          | Datuaren jatorrizko instantzia (lehenetsia `DEFAULT`) |
| `popMessageAfterSync`    | `PopMessageAfterSyncAdminSpec`| ❌          | Sinkronizazioaren ondoren bezeroan erakusteko mezu aukerakoa |

---

## Erantzun arrakastatsua (HTTP 200)

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

> Erantzunaren `payload` zuzenean `DN00SyncMetaDataFromAdminCOREProcessingInfo` objektua da (`@MarshallType(as="syncMetaDataFromAdminCOREProcessingInfo")`); **ez dago `processingInfo` bilgarririk**. `code`-a (prozesamenduaren egoera, `OK`/`CLIENT_ERR`/`SERVER_ERR`/`QUEUED` balioak) erro-mailan doa, `context` eta `payload`-en anaia.

| Eremua | Mota | Deskribapena |
|--------|------|--------------|
| `transactionOid` | `String` | CORE-k sinkronizazio honetarako esleitutako transakzio-OID |
| `receivedItemsCount` | `Number` | Jasotako elementu kopurua |
| `processedOK` | `Array` | Zuzen prozesatutako elementuak (SRMD) |
| `processedNOK` | `Array` | Huts egin duten elementuak, bakoitza `item` (jatorrizko elementua) eta `error` (arrazoia) dituela |

## Errore-erantzuna (HTTP 4xx/5xx)

```
Error: The items to be synced cannot be null or empty
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
