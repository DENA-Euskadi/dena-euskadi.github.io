# PERSON-SYNC — Pertsonen Kontsulta (Search)

## Endpoint

```
POST /person/api/admin/persons
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

## Deskribapena

Iragazki bat betetzen duten pertsonak bilatzen ditu (pertsona-erreferentziak eta/edo sortze/eguneratze data-tarte bat) eta bat datozen pertsonen **bilduma paginatu** bat itzultzen du. `person-admin-rest` zerbitzuaren **Pertsonen Kontsulta**ren parte da.

## Request

```json
{
    "context": {
        "message": {
            "type": "PERSON_ADMIN_SEARCH",
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
        "filter": {
            "ref": [
                { "oid": "96C615E2-B24E-4449-B6B4-9551A4EE0A9D" }
            ],
            "dateRange": "Instant:[2026-01-01T00:00:00Z..2026-12-31T23:59:59Z]"
        },
        "paginationRequest": {
            "firstItemPosition": 0,
            "requestedNumberOfItems": 25
        }
    }
}
```

| Eremua    | Mota                                           | Nahitaezkoa | Deskribapena |
|-----------|------------------------------------------------|:-----------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅          | Eskaeraren testuinguru-objektua, `message.type` eremuan `PERSON_ADMIN_SEARCH` balioarekin |
| `payload` | [Payload](#payload)                            | ✅          | Bilaketa-iragazkia eta paginazio-eskaera |

## Payload

| Eremua              | Mota       | Nahitaezkoa | Deskribapena |
|---------------------|------------|-------------|-------------|
| `filter`            | `Object`   | ✅          | Bilaketa-irizpideak |
| `filter.ref`        | `Array<[PersonRef](../../../semantica-base/modelo/person-ref.md)>` | ❌ | Bilatu beharreko pertsonen erreferentziak (`oid` eta/edo `id`) |
| `filter.dateRange`  | `String`   | ❌          | Sortze/eguneratze tartea `Instant:[<hasiera>..<amaiera>]` formatuan |
| `paginationRequest` | `Object`   | ✅          | Paginazio-eskaera (`firstItemPosition`, `requestedNumberOfItems`) |

## Response arrakastatsua (HTTP 200)

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "PERSON_ADMIN_SEARCH",
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
        "totalItemsCount": 1,
        "startPosition": 0,
        "endPosition": 1,
        "requestedNumberOfItems": 25,
        "pageItems": [
            {
                "person": {
                    "oid": "96C615E2-B24E-4449-B6B4-9551A4EE0A9D",
                    "id": "99949992L",
                    "name": "NOMBRE",
                    "surname1": "APELLIDOUNO",
                    "surname2": "ACTIVO"
                }
            }
        ]
    }
}
```

| Eremua | Mota | Deskribapena |
|--------|------|-------------|
| `totalItemsCount` | `Number` | Iragazkia betetzen duten pertsonen guztizkoa |
| `startPosition` / `endPosition` | `Number` | Itzulitako orriaren posizioak |
| `requestedNumberOfItems` | `Number` | Eskatutako orri-tamaina |
| `pageItems` | `Array` | Emaitzak; elementu bakoitzak pertsona bat biltzen du `person` eremuan |

## Errore-response (HTTP 4xx/5xx)

```json
{
  "message" : "The search filter is invalid",
  "code" : -9999,
  "path" : "/person/api/admin/persons"
}
```

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Bilaketa exekutatu da; emaitzen orri bat (hutsa izan daiteke) itzultzen da |
| `400` | Eskaera gaizki eratua, iragazki baliogabea edo traffic-flow goiburuak falta |
| `401` | Baimenik gabe (token baliogabea edo iraungia) |
| `403` | Debekatua (baimenik gabe) |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
