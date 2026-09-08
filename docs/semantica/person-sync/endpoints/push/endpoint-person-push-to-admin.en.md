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
    "message": {
      "type": "PERSON_PUSH_TO_ADMIN",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": [
        { "denaComponentId": "apiGateway", "timestamp": "2024-06-01T10:00:00Z" }
      ]
    },
    "destinationAdmin": { "oid": "6AE83A0C-2202-4666-9857-3334C14663A2", "id": "ADMIN-001", "dir3Id": "EA0000001" },
    "subjectPerson": { "id": "12345678A", "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C" }
  },
  "payload": {
    "personRef": {
        "id": "12345678A",
        "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C"
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
| `context` | [Context](../../../semantica-base/index.md) | ✅        | Request context object. Includes `message.type`, `destinationAdmin` (OrgAdminRef) and `subjectPerson` (PersonRef) |
| `payload` | [Payload](#payload)                            | ✅        | Request payload |

## Payload

| Field            | Type     | Mandatory | Description |
|------------------|----------|:---------:|-------------|
| `personRef`      | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Reference to the created or modified person |
| `personHashes`   | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Hashes of the person's name and surnames for unambiguous identification |
| `createDate`     | `ISO 8601 Date` | ✅ | Creation date |
| `lastUpdateDate` | `ISO 8601 Date` | ❌ | Last update date |
| `syncEvent`      | `String` | ✅ | Event that occurred. Possible values: <br> `CREATED`: New person registered <br> `DELETED`: Person removed from DENA <br> `UPDATED`: Person's data updated <br> `ID_CHANGED`: Person's identifier modified |

!!! note "About `message.type`"
    The value `PERSON_PUSH_TO_ADMIN` does not yet exist in the `DN00InteropMessageType` enum of code 0.4.16 (the defined types are ADMIN → DENA-CORE flows). It is kept here as the identifier for the DENA-CORE → administration flow, pending the addition of the corresponding real type.

---

## Successful response (HTTP 200)

```json
{
  "context": {
    "message": {
      "type": "PERSON_PUSH_TO_ADMIN",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C" }
  },
  "payload": null,
  "code": "OK"
}
```

## Error response (HTTP 4xx/5xx)

```json
{
  "context": {
    "message": {
      "type": "PERSON_PUSH_TO_ADMIN",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C" }
  },
  "payload": null,
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
2. Interpret `payload.personRef` to identify the person
3. Update its database of persons registered in DENA with the received information
4. Respect standard HTTP codes
5. Respond in less than 30 seconds
6. Use `code: "OK"` in successful responses and `code: "CLIENT_ERR"` or `code: "SERVER_ERR"` in errors

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
