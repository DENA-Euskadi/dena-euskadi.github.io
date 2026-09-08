# PERSON-SYNC — Get Pull from Admin Bespoke Job

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes/12345-ABC-DEFG
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

## Deskribapena

Esportazio-eskaera baten egoera kontsultatzen du.

## Eskaera

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_PULL_BESPOKE_FETCH",
            "correlationId": "aa645a6e-66a0-4c02-a00f-81d484a4296a",
            "interopRouteData": [
                {
                    "denaComponentId": "DENA_POSTMAN",
                    "timestamp":"2026-06-11T14:55:01.7520000Z"
                }
            ]
        },
        "originAdmin": {
            "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
            "id": "admin-A414",
            "dir3Id": "EA0000001"
        },
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36"
    },
    "payload": {
        "jobOid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
    }
}
```

| Eremua    | Mota                                           | Derrigorrez | Deskribapena |
|-----------|------------------------------------------------|:-----------:|--------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, `ADMIN_PERSON_PULL_BESPOKE_FETCH` balioarekin `message.type` barne |
| `payload` | [Payload](#payload)                            | ✅          | Eskaeraren payload-a |


## Payload

| Eremua   | Mota     | Derrigorrez | Deskribapena |
|----------|----------|:-----------:|--------------|
| `jobOid` | `String` | ✅          | Egoera kontsultatu beharreko eskaeraren identifikatzailea |

## Erantzun arrakastatsua (HTTP 200)

```json
{
    "payload": {
        "job": {
            "admin": {
                "id": "admin-A414",
                "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
            },
            "entityVersion": 1,
            "exportResultAssets": [
                {
                    "exportContentSpec": {
                        "personExportSpec": "sync",
                        "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
                        "syncEvent": "CREATED",
                        "exportFileFormat": "CSV"
                    },
                    "fileStoreItemOid": "652DB18C-DBD6-4B6C-A520-7D808A3CB8FE"
                }
            ],
            "exportSpec": {
                "personExportSpec": "sync",
                "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
                "exportFileFormat": "CSV"
            },
            "finishedAt": "2026-05-26T23:28:00.1193311Z",
            "numericId": 0,
            "oid": "0E6A859E-2472-4A86-9111-2AB7DB70DB9B",
            "processingAttempts": 1,
            "registeredAt": "2026-05-26T23:26:06.2436446Z",
            "startedAt": "2026-05-26T23:28:00.0384188Z",
            "status": "FINISHED_OK",
            "trackingInfo": {
                "createDate": "2026-05-26T23:26:06.2436450Z",
                "creatorUserCode": "mock-user-login",
                "creatorUserOid": "983cf052-e4fc-4a45-86de-3d53f4518813",
                "lastUpdate": "2026-05-26T23:28:00.1234540Z",
                "lastUpdaterUserCode": "mock-user-login",
                "lastUpdaterUserOid": "6ed87233-57e3-4d1e-80e7-90eba68db06a"
            }
        }
    }
}
```

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "message" : "The job with oid=F74724F6-65F6-4E01-B215-AB8CDA3FC42B does NOT exist",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes/F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
}
```

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Datuak zuzen itzulita (zerrenda hutsa izan daiteke) |
| `400` | Eskaera gaizki osatua edo parametro baliogabeak |
| `401` | Baimenik gabe (tokena baliogabea edo iraungita) |
| `403` | Debekatuta (baimenik gabe) |
| `404` | Pertsona ez da aurkitu |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago eskuragarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
