# PERSON-SYNC — Person Query (Fetch)

## Endpoint

Two variants depending on whether you identify the person by their **OID** (DENA stable identifier) or by their **ID** (NIF/NIE):

```
POST /person/api/admin/persons/{oid}
POST /person/api/admin/persons/byId/{id}
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

## Description

Retrieves the **full data** of a person registered in DENA. It is part of the **Person Query** (real-time REST query) of the `person-admin-rest` service. Use the `oid` variant whenever possible (it is stable); use the `id` variant when you only have the NIF/NIE.

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

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅        | Request context object, with `message.type` set to `PERSON_FETCH_DATA` |
| `payload` | [Payload](#payload)                            | ✅        | Reference to the person to retrieve |

## Payload

| Field | Type     | Mandatory | Description |
|-------|----------|-----------|-------------|
| `oid` | `String` | ✅ (OID variant) | Person OID in DENA |
| `id`  | `String` | ✅ (ID variant)  | Person NIF/NIE |

!!! note "Which field goes in the payload"
    In the `/{oid}` variant the `payload` carries `oid`; in the `/byId/{id}` variant the `payload` carries `id`. The same value also goes in the path.

## Successful response (HTTP 200)

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

The response `payload` is a [`DN00Person`](../../../../semantica/data-retrieve/data/persona.md) object: `oid`, `id`, `name`, `surname1`, `surname2` and, when available, `contactInfo` and `lastChangeEvent`.

## Error response (HTTP 4xx/5xx)

```json
{
  "message" : "No person exists with oid=96C615E2-B24E-4449-B6B4-9551A4EE0A9D",
  "code" : -9999,
  "path" : "/person/api/admin/persons/96C615E2-B24E-4449-B6B4-9551A4EE0A9D"
}
```

---

## HTTP codes

| Code | Meaning |
|------|---------|
| `200` | Person found and returned |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorised (invalid or expired token) |
| `403` | Forbidden (insufficient permissions) |
| `404` | No person exists with the given OID/ID |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
