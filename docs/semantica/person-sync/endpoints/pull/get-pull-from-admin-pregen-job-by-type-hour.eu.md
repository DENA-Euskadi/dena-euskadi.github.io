# PERSON-SYNC — Get Pull from Admin Pregen Job (mota eta orduaren arabera)

Aurrez sortutako *job* bat kokatzen du **bere OIDa jakin gabe**, fitxategi-mota eta eguneko ordua adieraziz.

---

## Noiz erabiltzen da?

Hau da aurrez sortutako fitxategiekin lan egiteko ohiko modua: DENAk orduro automatikoki sortzen dituenez, zure administrazioak ez daki aldez aurretik beren OIDak. Endpoint honekin DENAri eskatzen diozu "emadazu *H* orduko *X* motako aurrez sortutakoa" eta bere metadatuak lortzen dituzu (egoera barne), gero [Fetch Persons Pregen Export Asset](./fetch-persons-pregen-export-asset.md)-ekin deskargatzeko.

## Endpointa

```
POST /person-sync/api/admin/persons/sync/pregens
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

!!! note "Biderik ez du `{jobOid}`"

    [Get Pull from Admin Pregen Job (OIDaren arabera)](./get-pull-from-admin-pregen-job.md)-en ez bezala, hemen bideak **ez** darama `jobOid`: job-a `payload`-aren eremuen bidez identifikatzen da (`jobType`, `exportType`, `fileFormat`, `hourOfDay`).

## Eskaera (Request)

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_PREGEN_JOB_FETCH_BY_TYPE_AND_HOUR",
            "correlationId": "0777f936-4c31-43b5-81ee-fdf4d708f147",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-06-10T15:37:57.5530000Z"
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
        "jobType": "ALL_PERSONS",
        "exportType": "SYNC",
        "fileFormat": "CSV",
        "hourOfDay": "01"
    }
}
```

| Eremua    | Mota                                          | Beharrezkoa | Deskribapena |
|-----------|-----------------------------------------------|-------------|--------------|
| `context` | [Context](../../../semantica-base/index.md)   | ✅          | Eskaeraren testuingurua, `message.type` `ADMIN_PERSON_PREGEN_JOB_FETCH_BY_TYPE_AND_HOUR` balioarekin barne |
| `payload` | [Payload](#payload)                           | ✅          | Eskaeraren payload-a |

## Payload

| Eremua       | Mota     | Beharrezkoa | Deskribapena |
|--------------|----------|-------------|--------------|
| `jobType`    | `String` | ✅          | Aurrez sortutako fitxategi-mota: <br> `ALL_PERSONS`: ordu horretako pertsona guztiak <br> `UPDATED_PERSONS_SINCE_LAST_SUCCESSFUL_JOB`: arrakastaz prozesatutako azken job-etik eguneratutako pertsonak soilik |
| `exportType` | `String` | ✅          | `data` (pertsona bakoitzaren datu guztiak) edo `sync` (sortze/eguneratze denbora-markak soilik) |
| `fileFormat` | `String` | ✅          | Formatua: `SQLITE`, `CSV`, `ZIP_OF_JSON` edo `PARQUET` |
| `hourOfDay`  | `String` | ❌          | Aldaketak gertatu ziren eguneko ordua (`00`–`23`) |

## Erantzun arrakastatsua (HTTP 200)

```json
{
    "payload": {
        "job": {
            "oid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B",
            "registeredAt": "2026-05-26T01:00:00.0284171Z",
            "status": "FINISHED_OK"
        }
    }
}
```

Itzulitako `oid` asset-a deskargatzeko edo job-a [OIDaren arabera](./get-pull-from-admin-pregen-job.md) kontsultatzeko erabil dezakezuna da. [Job-aren egoerak](./get-pull-from-admin-pregen-job.md#job-aren-egoerak) berdinak dira.

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "message": "No pregen job found for jobType=ALL_PERSONS hourOfDay=01",
  "code": -9999,
  "path": "/person-sync/api/admin/persons/sync/pregens"
}
```

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Job-a behar bezala itzuli da |
| `400` | Eskaera gaizki osatua edo parametro baliogabeak |
| `401` | Baimenik gabe (token baliogabea edo iraungia) |
| `403` | Debekatua (baimenik ez) |
| `404` | Ez dago aurrez sortutakorik adierazitako irizpideetarako |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
