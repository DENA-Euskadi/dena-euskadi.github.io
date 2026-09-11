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

The request body is a `DN00PersonSyncPushToAdminFromCOREToConnectorInternalSide` object (`@MarshallType(as="personSyncPushToAdminFromCOREToConnectorInternalSide")`), with the data origin configuration and the **notification** containing the person's data:

```json
{
  "dataOriginConfigForDataTypeInAdmin": { "...": "internal data origin configuration (connector use)" },
  "notification": {
    "syncData": {
      "personRef": {
        "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C",
        "id": "12345678A"
      },
      "personHashes": {
        "nameHash": "abcde",
        "surname1Hash": "abcde",
        "surname2Hash": "abcde",
        "fullNameHash": "abcde"
      },
      "createDate": "2024-06-01T10:00:00Z",
      "lastUpdateDate": "2024-06-01T10:00:00Z",
      "syncEvent": "CREATED"
    },
    "person": {
      "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C",
      "id": "12345678A",
      "name": "Ane",
      "surname1": "Garcia",
      "surname2": "Lopez",
      "contactInfo": { "...": "contact data (ContactInfo)" },
      "lastChangeEvent": "CREATED"
    }
  }
}
```

| Field | Type | Mandatory | Description |
|-------|------|:---------:|-------------|
| `dataOriginConfigForDataTypeInAdmin` | `Object` | ✅ | Data origin configuration for the data type in the administration. Internal information used by the connector; the administration does not need to interpret it |
| `notification` | `DN00PersonSyncPushToAdminNotification` (`@MarshallType(as="personSyncPushToAdminNotification")`) | ✅ | Notification with the person's data to synchronize |

## `notification`

| Field | Type | Mandatory | Description |
|-------|------|:---------:|-------------|
| `syncData` | `DN00PersonSyncData` (`@MarshallType(as="personSyncData")`) | ✅ | Synchronization metadata (reference, hashes, dates, event) |
| `person` | `DN00Person` (`@MarshallType(as="person")`) | ✅ | Full person data |

### `notification.syncData`

| Field | Type | Mandatory | Description |
|-------|------|:---------:|-------------|
| `personRef` | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Reference to the created or modified person (`oid`/`id`) |
| `personHashes` | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Hashes of name and surnames for unambiguous identification |
| `createDate` | `Instant` (ISO 8601) | ❌ | Creation date |
| `lastUpdateDate` | `Instant` (ISO 8601) | ❌ | Last update date |
| `syncEvent` | `DN00PersonChangeEvent` | ❌ | Event that triggered the sync: `CREATED` (new person), `DELETED` (person removed), `UPDATED` (data updated), `ID_CHANGED` (identifier modified) |

### `notification.person`

| Field | Type | Mandatory | Description |
|-------|------|:---------:|-------------|
| `oid` / `id` | `String` | ✅ | Person identifiers |
| `name` | `String` | ✅ | Name |
| `surname1` | `String` | ✅ | First surname |
| `surname2` | `String` | ❌ | Second surname |
| `contactInfo` | `ContactInfo` | ❌ | Contact data |
| `lastChangeEvent` | `DN00PersonChangeEvent` | ❌ | Last change type applied (set by DENA-CORE) |

---

## Response

The administration signals the processing result through the **HTTP status code**:

- **`200 OK`** — the notification was processed successfully. A body is not required.
- **`4xx`** — error attributable to the request (e.g. `404` if the person cannot be resolved, `400` if the body is invalid).
- **`5xx`** — internal error at the administration.

DENA-CORE interprets the result from the HTTP code (see `DN01PersonPushToAdminJobProcessor`): if the response is successful the job moves to `SYNCED_OK`; otherwise it is retried (up to the maximum number of attempts) and moves to `SYNCED_ERROR` / `SYNCED_ERROR_TOO_MANY_ATTEMPTS`.

If the administration returns an error body, a simple object with a descriptive message is recommended, for example:

```json
{
  "error": "PERSON_NOT_FOUND",
  "message": "Person not found in the system"
}
```

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
2. Interpret `notification.syncData.personRef` (and `notification.person`) to identify the person
3. Update its database of persons registered in DENA with the received information
4. Respect standard HTTP codes (`200` if processed successfully; `4xx`/`5xx` on error)
5. Respond in less than 30 seconds

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
