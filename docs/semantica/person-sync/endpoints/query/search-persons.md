# PERSON-SYNC — Consulta de Personas (Search)

## Endpoint

```
POST /person/api/admin/persons
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

## Descripción

Busca personas que cumplan un filtro (referencias de persona y/o un rango de fechas de creación/actualización) y devuelve una **colección paginada** de personas coincidentes. Forma parte de la **Consulta de Personas** del servicio `person-admin-rest`.

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

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅          | Objeto de contexto de la petición, con `message.type` con valor `PERSON_ADMIN_SEARCH` |
| `payload` | [Payload](#payload)                            | ✅          | Filtro de búsqueda y petición de paginación |

## Payload

| Campo               | Tipo       | Obligatorio | Descripción |
|---------------------|------------|-------------|-------------|
| `filter`            | `Object`   | ✅          | Criterios de búsqueda |
| `filter.ref`        | `Array<[PersonRef](../../../semantica-base/modelo/person-ref.md)>` | ❌ | Referencias (`oid` y/o `id`) de las personas a buscar |
| `filter.dateRange`  | `String`   | ❌          | Rango de creación/actualización en formato `Instant:[<desde>..<hasta>]` |
| `paginationRequest` | `Object`   | ✅          | Petición de paginación (`firstItemPosition`, `requestedNumberOfItems`) |

## Response exitosa (HTTP 200)

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

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `totalItemsCount` | `Number` | Total de personas que cumplen el filtro |
| `startPosition` / `endPosition` | `Number` | Posiciones de la página devuelta |
| `requestedNumberOfItems` | `Number` | Tamaño de página solicitado |
| `pageItems` | `Array` | Resultados; cada elemento envuelve una persona en el campo `person` |

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "The search filter is invalid",
  "code" : -9999,
  "path" : "/person/api/admin/persons"
}
```

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Búsqueda ejecutada; se devuelve una página (posiblemente vacía) de resultados |
| `400` | Petición malformada, filtro inválido o cabeceras del traffic-flow ausentes |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `500` | Error interno |
| `503` | Servicio no disponible |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
