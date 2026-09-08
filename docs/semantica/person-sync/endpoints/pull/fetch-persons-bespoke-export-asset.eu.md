# PERSON-SYNC — Fetch Persons Bespoke Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

## Deskribapena

DENAren erabiltzaileen esportazio-eskaera baten emaitza deskargatzen du adierazitako formatuan.

## Eskaera

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH",
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
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` balioarekin `message.type` barne |
| `payload` | [Payload](#payload)                            | ✅          | Eskaeraren payload-a |


## Payload

| Eremua   | Mota     | Derrigorrez | Deskribapena |
|----------|----------|:-----------:|--------------|
| `jobOid` | `String` | ✅          | Emaitza deskargatu beharreko eskaeraren identifikatzailea |

## Erantzun arrakastatsua (HTTP 200)

Eskatutako formatuko erabiltzaile-esportazio fitxategiaren datu binarroak.

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes/F74724F6-65F6-4E01-B215-AB8CDA3FC42B/asset"
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
