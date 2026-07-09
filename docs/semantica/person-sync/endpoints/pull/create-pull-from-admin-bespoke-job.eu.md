# PERSON-SYNC — Create Pull from Admin Bespoke Job

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

## Deskribapena

DENAren erabiltzaileen zerrenda baten esportazio-eskaera sortzen du modu asinkronoan prozesatzeko.

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
        "messageType": "CREATE_PULL_ADMIN_BESPOKE",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originPartyId": "ADMIN-001",
        "destinationPartyId": "DENA_INTEROP"
    },
    "data": {
        "orgAdminRef": {
            "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
        },
        "exportSpec": {
            "personExportSpec": "sync",
            "exportFileFormat": "CSV",
            "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
            "syncEvent": "CREATED"
        }
    }
}
```

| Eremua    | Mota                                           | Derrigorrez | Deskribapena |
|-----------|------------------------------------------------|:-----------:|--------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, `CREATE_PULL_ADMIN_BESPOKE` balioarekin messageType barne |
| `data`    | [Data](#data)                                  | ✅          | Eskaeraren payload-a |


## Data

| Eremua        | Mota     | Derrigorrez | Deskribapena |
|---------------|----------|:-----------:|--------------|
| `orgAdminRef` | [OrgAdminRef](../../../semantica-base/modelo/org-admin-ref.md) | ✅ | Administrazioaren erreferentzia |
| `exportSpec`  | [ExportSpec](../../modelo/pull/export-spec.md) | ✅ | Esportatu beharreko pertsonen eta helburu-formatuaren zehaztapena |

## Erantzun arrakastatsua (HTTP 200)

```json
{
    "data": {
        "job": {
            "admin": {
                "id": "admin-A414",
                "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
            },
            "entityVersion": 0,
            "exportSpec": {
                "personExportSpec": "sync",
                "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
                "syncEvent": "CREATED",
                "exportFileFormat": "CSV"
            },
            "numericId": 0,
            "oid": "7F4C8F2A-03AB-4864-AC19-7D2E2FAD7FC2",
            "processingAttempts": 0,
            "registeredAt": "2026-05-26T23:23:28.0284171Z",
            "status": "REGISTERED",
            "trackingInfo": {
                "createDate": "2026-05-26T23:23:28.4973937Z",
                "creatorUserCode": "mock-user-login",
                "creatorUserOid": "09587694-e8ad-4d45-8f0c-0000f753d716",
                "lastUpdate": "2026-05-26T23:23:28.4973937Z"
            }
        }
    }
}
```

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "message" : "400 BAD_REQUEST \"request doesn't contain neither [admin]  oid or id \"",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes"
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
