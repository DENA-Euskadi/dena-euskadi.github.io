# Errors and Troubleshooting

## Description

Guide to common errors when implementing the DATA-RETRIEVE endpoint and how to resolve them.

---

## Request errors (received by the administration)

### 400 — Malformed request

| Cause | Solution |
|-------|----------|
| Invalid JSON in the body | Validate that the body is valid JSON before processing it |
| Missing `context.subjectPerson.personId` | Mandatory field — verify it is received |
| Missing `context.dataType.dataTypeId` | Mandatory field — verify it is received |
| `dataTypeId` with unrecognised value | Accept only: `RECORDS`, `NOTICES`, `REGISTER`, `PAYMENTS`, `SCHEDULE`, `PERSON_DATA` |

### 401 — Unauthorised

| Cause | Solution |
|-------|----------|
| OAuth2 token absent | Verify that the `Authorization: Bearer <token>` header is present |
| Expired token | DENA renews tokens automatically; if it persists, review the client credentials configuration |
| Invalid token | Verify that the endpoint validates against the same authorisation server configured in DENA |

### 404 — Person not found

| Cause | Solution |
|-------|----------|
| The `personId` does not exist in the administration's system | Return HTTP 404 with a standard error body |
| Unrecognised `personId` format | Accept DNI (8 digits + letter), NIE (X/Y/Z + 7 digits + letter) and NIF |

---

## Response errors (returned by the administration)

### Standard error structure

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
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

| Field | Description |
|-------|-------------|
| `code` | `CLIENT_ERR` (client error) or `SERVER_ERR` (server error) |
| `errorId` | Specific error code defined by the administration (optional) |
| `details.details` | Descriptive error message |

### Status codes (`code`)

| Code | When to use it |
|------|----------------|
| `OK` | Successful response (even with an empty list) |
| `CLIENT_ERR` | Malformed request, person not found, invalid parameters |
| `SERVER_ERR` | Internal error on the administration's server |
| `QUEUED` | The message has been queued for asynchronous processing |

### Common format errors in `dataItems`

| Error | Symptom in DENA | Solution |
|-------|-----------------|----------|
| Missing `type` field in an object | DENA cannot deserialise the object | Always include the `type` field with the correct value |
| Date in incorrect format | Parsing error | Use ISO 8601: `2024-03-15T10:30:00Z` |
| `LocalDate` date with time | Parsing error | Use only `YYYY-MM-DD` for date fields without time |
| `LanguageTexts` with invalid language key | Text is not displayed | Use `SPANISH`, `BASQUE` or `ENGLISH` (uppercase) |
| `amountInEuro` as a string | Type error | Send as a number: `45.50`, not `"45.50"` |
| `urls` as an object instead of an array | Deserialisation error | `urls` is always an array, even with a single element |
| `state` as a string in a record | Type error | In records and registrations, `state` is an object with `stateCode` and `description` |
| `state` as an object in a notification | Type error | In notifications, `state` is directly a string (the status code) |

---

## Connectivity issues

| Problem | Diagnosis | Solution |
|---------|-----------|----------|
| DENA cannot connect to the endpoint | Timeout or connection refused | Verify that the endpoint is accessible from the DENA network |
| Invalid SSL certificate | TLS handshake error | Use a valid certificate issued by a recognised CA |
| Response too slow | Timeout (30s by default) | Optimise queries; consider pagination if there is a lot of data |
| Response too large | Memory error | Limit `dataItems` to a reasonable maximum (< 1000 elements) |

---

## Data issues

| Problem | Symptom | Solution |
|---------|---------|----------|
| Record without service or procedure | Object rejected by validation | `service` and `procedure` are mandatory in records |
| Payment with `forStatus: COMPLETED` but without `paidAt` | Data inconsistency | If the payment is completed, include `paymentDates.paidAt` |
| Notification with `readedAt` but status `PENDING` | Data inconsistency | If it has a read date, the status must be `ACKNOWLEDGED` or `REJECTED` |
| Appointment with negative `durationMinutes` | Validation failed | Use 0 for milestones, positive values for appointments with a duration |
| Direct debit without `directDebitData` | Incomplete object | The `directDebitData` block is mandatory for direct debits |

---

## Implementation checklist

Before connecting to DENA, verify:

- [ ] The endpoint accepts `POST` with `Content-Type: application/json`
- [ ] The endpoint returns `Content-Type: application/json`
- [ ] `context.subjectPerson.personId` is correctly interpreted
- [ ] `context.dataType.dataTypeId` is correctly interpreted
- [ ] All objects in `dataItems` include the `type` field
- [ ] Multilingual texts include at least `SPANISH` and `BASQUE`
- [ ] Dates are in ISO 8601 format
- [ ] Amounts are numbers (not strings)
- [ ] URLs are valid HTTPS
- [ ] `{ "data": { "dataItems": [] }, "code": "OK" }` is returned when there is no data
- [ ] HTTP 200 is returned even when the list is empty
- [ ] HTTP error codes are used correctly (400, 401, 404, 500)
- [ ] `code: "OK"` is used on success and `code: "CLIENT_ERR"` / `code: "SERVER_ERR"` on errors
- [ ] The endpoint responds in less than 30 seconds

---

## Contact and support

If the error persists after reviewing this guide, contact the DENA team providing:

1. The `messageCorrelationId` of the request
2. The HTTP code returned
3. The response body (if applicable)
4. Server logs with timestamp

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
