# PERSON-SYNC — Get Pull from Admin Pregen Job (OIDaren arabera)

Aurrez sortutako *job* baten egoera eta metadatuak kontsultatzen ditu bere identifikatzailetik (`jobOid`).

---

## Noiz erabiltzen da?

**Aurrez sortutako** fitxategiak DENAk automatikoki sortzen ditu orduro (administrazioak ez ditu eskatzen). Endpoint honek **aurrez sortutako job zehatz bat kontsultatzeko** aukera ematen du bere `jobOid` jada ezagutzen duzunean, bere egoera eta zer esportazio duen jakiteko [Fetch Persons Pregen Export Asset](./fetch-persons-pregen-export-asset.md)-ekin deskargatu aurretik.

!!! tip "Ez dakizu OIDa?"

    `jobOid` ez baduzu eta ordu/mota zehatz baten aurrez sortutakoa kokatu nahi baduzu, erabili [Get Pull from Admin Pregen Job (mota eta orduaren arabera)](./get-pull-from-admin-pregen-job-by-type-hour.md).

## Endpointa

```
POST /person-sync/api/admin/persons/sync/pregens/{jobOid}
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

Non `{jobOid}` kontsultatu nahi duzun aurrez sortutako job-aren identifikatzailea den.

## Eskaera (Request)

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_PREGEN_JOB_FETCH",
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
        "jobOid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
    }
}
```

| Eremua    | Mota                                          | Beharrezkoa | Deskribapena |
|-----------|-----------------------------------------------|-------------|--------------|
| `context` | [Context](../../../semantica-base/index.md)   | ✅          | Eskaeraren testuingurua, `message.type` `ADMIN_PERSON_PREGEN_JOB_FETCH` balioarekin barne |
| `payload` | [Payload](#payload)                           | ✅          | Eskaeraren payload-a |

## Payload

| Eremua   | Mota     | Beharrezkoa | Deskribapena |
|----------|----------|-------------|--------------|
| `jobOid` | `String` | ✅          | Kontsultatu beharreko aurrez sortutako job-aren identifikatzailea |

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

| Job-aren eremua | Deskribapena |
|-----------------|--------------|
| `oid` | Aurrez sortutako job-aren identifikatzailea |
| `registeredAt` | DENAk job-a erregistratu/sortu zuen unea |
| `status` | Job-aren egoera (ikus [Egoerak](#job-aren-egoerak)) |

## Job-aren egoerak

| Egoera | Esanahia |
|--------|----------|
| `REGISTERED` | Erregistratua, prozesatzeke |
| `BEING_PROCESSED` | Sortze-prozesuan |
| `FINISHED_OK` | Behar bezala amaitua: fitxategia deskargatzeko prest dago |
| `FINISHED_NOK` | Errorearekin amaitua |
| `FINISHED_NOK_TOO_MANY_ATTEMPTS` | Errorearekin amaitua saiakerak agortu ondoren |

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "message": "The job with oid=F74724F6-65F6-4E01-B215-AB8CDA3FC42B does NOT exist",
  "code": -9999,
  "path": "/person-sync/api/admin/persons/sync/pregens/F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
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
| `404` | Job-a ez da aurkitu |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
