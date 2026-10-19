# PERSON-SYNC — Pertsonen Kontsulta (Head)

## Endpoint

Pertsona bat **existitzen den** egiaztatzen du bere datuak itzuli gabe. Bi aldaera, **OID**aren edo **ID**aren arabera:

```
POST /person/api/admin/persons/{oid}:head
POST /person/api/admin/persons/byId/{id}:head
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

!!! note "Aditz pertsonalizatua ibilbidean (AIP-136)"
    `:head` atzizkiak [AIP-136](https://google.aip.dev/136) konbentzioa jarraitzen du (Google API Improvement Proposals) ibilbideko metodo pertsonalizatuetarako. *head* deitzen bada ere, eragiketa `POST` bat da (eskaeraren testuingurua duen gorputza darama).

## Deskribapena

Pertsona bat DENAn **existitzen den** egiaztatzen du bere datu osoak berreskuratu gabe. Erabilgarria SRMD bidali aurreko egiaztapen azkarretarako. `person-admin-rest` zerbitzuaren **Pertsonen Kontsulta**ren parte da.

## Request

```json
{
    "context": {
        "message": {
            "type": "PERSON_ADMIN_HEAD",
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
| `context` | [Context](../../../semantica-base/index.md)    | ✅          | Eskaeraren testuinguru-objektua, `message.type` eremuan `PERSON_ADMIN_HEAD` balioarekin |
| `payload` | [Payload](#payload)                            | ✅          | Egiaztatu beharreko pertsonaren erreferentzia |

## Payload

| Eremua | Mota     | Nahitaezkoa | Deskribapena |
|--------|----------|-------------|-------------|
| `oid`  | `String` | ✅ (OID aldaera) | Pertsonaren OIDa DENAn |
| `id`   | `String` | ✅ (ID aldaera)  | Pertsonaren NAN/AIZ |

## Response arrakastatsua (HTTP 200)

Response-aren `payload`-a pertsona existitzen den adierazten duen **boolear** bat da:

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "PERSON_ADMIN_HEAD",
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
    "payload": true
}
```

| `payload` | Esanahia |
|-----------|----------|
| `true`    | Pertsona DENAn existitzen da |
| `false`   | Pertsona ez da DENAn existitzen |

## Errore-response (HTTP 4xx/5xx)

```json
{
  "message" : "The request could not be processed",
  "code" : -9999,
  "path" : "/person/api/admin/persons/96C615E2-B24E-4449-B6B4-9551A4EE0A9D:head"
}
```

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Egiaztapena egin da; `payload`-eko boolearrak existitzen den adierazten du |
| `400` | Eskaera gaizki eratua edo parametro baliogabeak |
| `401` | Baimenik gabe (token baliogabea edo iraungia) |
| `403` | Debekatua (baimenik gabe) |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
