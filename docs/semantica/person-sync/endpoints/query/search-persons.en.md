# PERSON-SYNC — Person Query (Search)

## Endpoint

```
POST /person/api/admin/persons
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

## Description

Searches for persons matching a filter (person references and/or a created/updated date range) and returns a **paginated collection** of matching persons. It is part of the **Person Query** of the `person-admin-rest` service.

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

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅        | Request context object, with `message.type` set to `PERSON_ADMIN_SEARCH` |
| `payload` | [Payload](#payload)                            | ✅        | Search filter and pagination request |

## Payload

| Field               | Type       | Mandatory | Description |
|---------------------|------------|-----------|-------------|
| `filter`            | `Object`   | ✅        | Search criteria |
| `filter.ref`        | `Array<[PersonRef](../../../semantica-base/modelo/person-ref.md)>` | ❌ | References (`oid` and/or `id`) of the persons to search |
| `filter.dateRange`  | `String`   | ❌        | Created/updated range in `Instant:[<from>..<to>]` format |
| `paginationRequest` | `Object`   | ✅        | Pagination request (`firstItemPosition`, `requestedNumberOfItems`) |

## Successful response (HTTP 200)

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

| Field | Type | Description |
|-------|------|-------------|
| `totalItemsCount` | `Number` | Total persons matching the filter |
| `startPosition` / `endPosition` | `Number` | Positions of the returned page |
| `requestedNumberOfItems` | `Number` | Requested page size |
| `pageItems` | `Array` | Results; each item wraps a person in the `person` field |

## Error response (HTTP 4xx/5xx)

```json
{
  "message" : "The search filter is invalid",
  "code" : -9999,
  "path" : "/person/api/admin/persons"
}
```

---

## HTTP codes

| Code | Meaning |
|------|---------|
| `200` | Search executed; a (possibly empty) page of results is returned |
| `400` | Malformed request, invalid filter or missing traffic-flow headers |
| `401` | Unauthorised (invalid or expired token) |
| `403` | Forbidden (insufficient permissions) |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
