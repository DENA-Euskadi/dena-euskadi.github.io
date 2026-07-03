# PERSON-SYNC — Fetch Persons Bespoke Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (if OAuth is configured)
```

## Description

Downloads the result of a DENA user export request in the specified format.

## Request

```json
{
    "context": {
        "interopRouteData": [
            {
                "denaComponentId": "DENA_POSTMAN",
                "timestamp":"2026-06-11T14:55:01.7520000Z"
            }
        ],
        "messageCorrelationId": "aa645a6e-66a0-4c02-a00f-81d484a4296a",
        "messageType": "FETCH_BESPOKE_EXPORT_ASSET",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originPartyId": "ADMIN-001",
        "destinationPartyId": "DENA_INTEROP"
    },
    "data": {
        "jobOid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
    }
}
```

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅        | Request context object, including messageType with value `FETCH_BESPOKE_EXPORT_ASSET` |
| `data`    | [Data](#data)                                  | ✅        | Request payload |


## Data

| Field    | Type     | Mandatory | Description |
|----------|----------|:---------:|-------------|
| `jobOid` | `String` | ✅        | Identifier of the request whose result is to be downloaded |

## Successful response (HTTP 200)

Binary data of the user export file in the requested format.

## Error response (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes/F74724F6-65F6-4E01-B215-AB8CDA3FC42B/asset"
}
```

---

## HTTP codes

| Code | Meaning |
|------|---------|
| `200` | Data returned successfully (may be an empty list) |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorised (invalid or expired token) |
| `403` | Forbidden (insufficient permissions) |
| `404` | Person not found |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
