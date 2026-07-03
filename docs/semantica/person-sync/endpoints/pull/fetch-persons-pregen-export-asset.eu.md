# PERSON-SYNC — Fetch Persons Pregen Export Asset

## Endpoint-a

```
POST /person-sync/api/admin/persons/sync/pregens/asset
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

## Deskribapena

DENA erabiltzaileen aurrez sortutako zerrenda bat deskargatzen du. Hauek eguneko ordu bakoitzean sortzen dira, ordu horretan gertatu diren pertsonen aldaketekin.

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
        "messageType": "FETCH_PREGEN_EXPORT_ASSET",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originPartyId": "ADMIN-001",
        "destinationPartyId": "DENA_INTEROP"
    },
    "data": {
        "jobType": "ALL_PERSONS",
        "exportType": "SYNC",
        "fileFormat": "CSV",
        "hourOfDay": "01"
    }
}
```

| Eremua    | Mota                                           | Nahitaezkoa | Deskribapena |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Eskaeraren testuinguru-objektua, messageType `FETCH_PREGEN_EXPORT_ASSET` balioarekin |
| `data`    | [Data](#data)                                  | ✅          | Eskaeraren payload-a |


## Data

| Eremua       | Mota     | Nahitaezkoa | Deskribapena |
|--------------|----------|-------------|-------------|
| `type`       | `String` | ✅          | `"fetchPreGenExportAssetRequestPayload"` |
| `jobType`    | `String` | ✅          | Aurrez sortutako fitxategi-mota. Balio posibleak: <br> `ALL_PERSONS`: Ordu bakoitzeko pertsona guztiak <br> `UPDATED_PERSONS_SINCE_LAST_SUCCESSFUL_JOB`: Ordu bakoitzean eguneratutako pertsonak soilik |
| `exportType` | `String` | ✅          | `data` (Pertsona bakoitzaren datu guztiak barne hartzen ditu) <br> `sync` (Sorrera/eguneraketa timestamp-ak soilik barne hartzen ditu) |
| `fileFormat` | `String` | ✅          | Datuak deskargatzeko formatua. Balio posibleak: `SQLITE`, `CSV`, `ZIP_OF_JSON` edo `PARQUET` |
| `hourOfDay`  | `String` | ❌          | Pertsonetan aldaketak gertatu diren eguneko ordua (00-23) |

## Erantzun arrakastatsua (HTTP 200)

Eskatutako formatuan erabiltzaileen esportazio-fitxategiaren datu bitarrak

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/pregens/asset"
}
```

---

## HTTP kodeak

| Kodea | Esanahia |
|--------|-------------|
| `200` | Datuak behar bezala itzuliak (zerrenda hutsa izan daiteke) |
| `400` | Eskaera gaizki osatua edo parametro baliogabeak |
| `401` | Baimenik gabe (token baliogabea edo iraungitua) |
| `403` | Debekatua (baimenik gabe) |
| `404` | Pertsona ez da aurkitu |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
