# Endpoint Person Push To Admin — Specification for Administrations

## Endpoint

```
POST /api/person/push
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

---

## Request

```json
{
  "context": {
    "messageType": "PERSON_PUSH_TO_ADMIN",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "REQUEST",
    "originPartyId": "DENA-CORE",
    "destinationPartyId": "ADMIN-001",
    "subjectPerson": { "personId": "12345678A" },
    "administration": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
    "interopRouteData": [
      { "denaComponentId": "apiGateway", "timestamp": "2024-06-01T10:00:00Z" }
    ]
  },
  "consentOid": "CONSENT-OID-2024-001",
  "data": {
    "personRef": {
        "id": "12345678A"
    },
    "personHashes": {
        "nameHash": "abcde",
        "surname1Hash": "abcde",
        "surname2Hash": "abcde",
        "allNamesHash": "abcde"
    },
    "createDate": "2024-06-01T10:00:00Z",
    "lastUpdateDate": "2024-06-01T10:00:00Z",
    "syncEvent": "CREATED"
  }
}
```

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../../../semantica-base/index.md) | ✅        | Request context object |
| `data`    | [Data](#data)                                  | ✅        | Request payload |

## Data

| Field            | Type     | Mandatory | Description |
|------------------|----------|:---------:|-------------|
| `personRef`      | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Reference to the created or modified person |
| `personHashes`   | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Hashes of the person's name and surnames for unambiguous identification |
| `createDate`     | `ISO 8601 Date` | ✅ | Creation date |
| `lastUpdateDate` | `ISO 8601 Date` | ❌ | Last update date |
| `syncEvent`      | `String` | ✅ | Event that occurred. Possible values: <br> `CREATED`: New person registered <br> `DELETED`: Person removed from DENA <br> `UPDATED`: Person's data updated <br> `ID_CHANGED`: Person's identifier modified |

---

## Successful response (HTTP 200)

```json
{
  "context": {
    "messageType": "PERSON_PUSH_TO_ADMIN",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "OK"
}
```

## Error response (HTTP 4xx/5xx)

```json
{
  "context": {
    "messageType": "PERSON_PUSH_TO_ADMIN",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Person not found in the system" }
}
```

### Status codes (`code`)

| Code | Description |
|------|-------------|
| `OK` | Message processed successfully |
| `CLIENT_ERR` | Client error (malformed request, person not found) |
| `SERVER_ERR` | Server error (internal error) |
| `QUEUED` | Message queued for asynchronous processing |

---

## Authentication

If the administration requires OAuth2, it will receive the header:

```
Authorization: Bearer <access_token>
```

The token is obtained automatically via client credentials.

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

---

## Requirements for the administration

1. Expose a `POST` endpoint that accepts and returns `application/json`
2. Interpret `data.personRef` to identify the person
3. Update its database of persons registered in DENA with the received information
4. Respect standard HTTP codes
5. Respond in less than 30 seconds
6. Use `code: "OK"` in successful responses and `code: "CLIENT_ERR"` or `code: "SERVER_ERR"` in errors

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
