# PERSON-SYNC — Pertsonen Kontsulta (Fetch)

## Endpoint

Bi aldaera, pertsona bere **OID**aren (DENAren identifikadore egonkorra) edo bere **ID**aren (NAN/AIZ) arabera identifikatzen duzun:

```
POST /person/api/admin/persons/{oid}
POST /person/api/admin/persons/byId/{id}
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

## Deskribapena

DENAn erregistratutako pertsona baten **datu osoak** berreskuratzen ditu. `person-admin-rest` zerbitzuaren **Pertsonen Kontsulta**ren (denbora errealeko REST kontsulta) parte da. Erabili `oid` aldaera ahal duzun guztietan (egonkorra da); erabili `id` aldaera NAN/AIZ soilik duzunean.

## Request

```json
{
    "context": {
        "message": {
            "type": "PERSON_FETCH_DATA",
            "correlationId": "c5e2726b-225e-4958-90fe-7cb9291bdbfa",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-05-16T10:15:30Z"
                }
            ]
        },
        "originAdmin": {
            "oid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011"
        }
    },
    "payload": {
        "oid": "96C615E2-B24E-4449-B6B4-9551A4EE0A9D"
    }
}
```

| Eremua    | Mota                                           | Nahitaezkoa | Deskribapena |
|-----------|------------------------------------------------|:-----------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅          | Eskaeraren testuinguru-objektua, `message.type` eremuan `PERSON_FETCH_DATA` balioarekin |
| `payload` | [Payload](#payload)                            | ✅          | Berreskuratu beharreko pertsonaren erreferentzia |

## Payload

| Eremua | Mota     | Nahitaezkoa | Deskribapena |
|--------|----------|-------------|-------------|
| `oid`  | `String` | ✅ (OID aldaera) | Pertsonaren OIDa DENAn |
| `id`   | `String` | ✅ (ID aldaera)  | Pertsonaren NAN/AIZ |

!!! note "Zein eremu doa payload-ean"
    `/{oid}` aldaeran `payload`-ak `oid` darama; `/byId/{id}` aldaeran `payload`-ak `id` darama. Balio bera doa ibilbidean ere.

## Response arrakastatsua (HTTP 200)

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "PERSON_FETCH_DATA",
            "correlationId": "c5e2726b-225e-4958-90fe-7cb9291bdbfa",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-05-16T10:15:30.0000000Z"
                },
                {
                    "denaComponentId": "DENA_CORE",
                    "timestamp": "2026-05-16T10:15:32.0000000Z"
                }
            ]
        }
    },
    "payload": {
        "oid": "96C615E2-B24E-4449-B6B4-9551A4EE0A9D",
        "id": "99949992L",
        "name": "NOMBRE",
        "surname1": "APELLIDOUNO",
        "surname2": "ACTIVO"
    }
}
```

Response-aren `payload`-a [`DN00Person`](../../../../semantica/data-retrieve/data/persona.md) objektu bat da: `oid`, `id`, `name`, `surname1`, `surname2` eta, eskuragarri badaude, `contactInfo` eta `lastChangeEvent`.

## Errore-response (HTTP 4xx/5xx)

```json
{
  "message" : "No person exists with oid=96C615E2-B24E-4449-B6B4-9551A4EE0A9D",
  "code" : -9999,
  "path" : "/person/api/admin/persons/96C615E2-B24E-4449-B6B4-9551A4EE0A9D"
}
```

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Pertsona aurkitu eta itzuli da |
| `400` | Eskaera gaizki eratua edo parametro baliogabeak |
| `401` | Baimenik gabe (token baliogabea edo iraungia) |
| `403` | Debekatua (baimenik gabe) |
| `404` | Ez dago pertsonarik OID/ID horrekin |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
