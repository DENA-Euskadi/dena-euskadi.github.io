# PERSON-SYNC — Person Query (Head)

## Endpoint

Checks whether a person **exists** without returning their data. Two variants, by **OID** or by **ID**:

```
POST /person/api/admin/persons/{oid}:head
POST /person/api/admin/persons/byId/{id}:head
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

!!! note "Custom verb in the path (AIP-136)"
    The `:head` suffix follows the [AIP-136](https://google.aip.dev/136) convention (Google API Improvement Proposals) for custom methods in the path. Even though it is named *head*, the operation is a `POST` (it carries a body with the request context).

## Description

Checks the **existence** of a person in DENA without retrieving their full data. Useful for quick checks before sending SRMD. It is part of the **Person Query** of the `person-admin-rest` service.

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

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅        | Request context object, with `message.type` set to `PERSON_ADMIN_HEAD` |
| `payload` | [Payload](#payload)                            | ✅        | Reference to the person to check |

## Payload

| Field | Type     | Mandatory | Description |
|-------|----------|-----------|-------------|
| `oid` | `String` | ✅ (OID variant) | Person OID in DENA |
| `id`  | `String` | ✅ (ID variant)  | Person NIF/NIE |

## Successful response (HTTP 200)

The response `payload` is a **boolean** indicating whether the person exists:

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

| `payload` | Meaning |
|-----------|---------|
| `true`    | The person exists in DENA |
| `false`   | The person does not exist in DENA |

## Error response (HTTP 4xx/5xx)

```json
{
  "message" : "The request could not be processed",
  "code" : -9999,
  "path" : "/person/api/admin/persons/96C615E2-B24E-4449-B6B4-9551A4EE0A9D:head"
}
```

---

## HTTP codes

| Code | Meaning |
|------|---------|
| `200` | Check performed; the boolean `payload` indicates whether the person exists |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorised (invalid or expired token) |
| `403` | Forbidden (insufficient permissions) |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
