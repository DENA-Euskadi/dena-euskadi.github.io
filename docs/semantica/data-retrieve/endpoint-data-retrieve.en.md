# Endpoint DATA-RETRIEVE — Specification for Administrations

## Endpoint

```
POST /api/retrieveData
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

!!! note "You choose the route"

    DENA does **not** impose a fixed path: it will `POST` to the **URL your administration configured** in DENA. `/api/retrieveData` is only an example (the one this documentation uses). The DENA **base connector** (`DN01ConnectorController`) exposes it at `/api/connector/retrieveData`, which you can take as a reference. What matters is accepting a `POST` with `application/json`.

---

## Request

The administration receives from the DENA connector a request with a `context` object (person, data type and administration) and a `payload` with the retrieval request. These are the relevant fields the administration must read:

```json
{
  "context": {
    "message": { "type": "PERSON_FETCH_DATA", "correlationId": "550e8400-e29b-41d4-a716-446655440000" },
    "originAdmin": { "id": "dena_connector" },
    "destinationAdmin": { "id": "ADMIN-001" },
    "subjectPerson": { "id": "12345678A" },
    "dataType": { "id": "administrativeServiceProcedureRecord" }
  },
  "payload": {
    "dataType": { "id": "administrativeServiceProcedureRecord" },
    "admin": { "id": "ADMIN-001" },
    "person": { "id": "12345678A" }
  }
}
```

| Field | Mandatory | Description |
|-------|:-----------:|-------------|
| `context.subjectPerson.id` | ✅ | DNI/NIE/NIF of the person whose data is requested |
| `context.dataType.id` | ✅ | Requested data type (marshallTypeId): `administrativeServiceProcedureRecord`, `administrativeNotice`, `administrativeOfficialRegisterRecord`, `oneOffPayment`, `directDebitPayment`, `scheduleItem`, `personData`. See [DataTypeRef](../semantica-base/modelo/data-type-ref.md) and [`DN00DataTypeEnum`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DataTypeEnum.java) |
| `context.destinationAdmin.id` | ✅ | Identifier of the destination administration (the one that serves the data) |
| `context.originAdmin.id` | ❌ | Identifier of the request origin (the DENA connector) |

!!! info "Field names"

    Inside `context` the fields are named `subjectPerson`, `dataType` and `destinationAdmin` (**not** `administration`). The inner `payload` repeats the request as `person` / `admin` / `dataType`. To implement the endpoint it is enough to read `context.subjectPerson.id` and `context.dataType.id` (and `context.destinationAdmin.id` if you serve several administrations).

---

## Successful response (HTTP 200)

> Response status: `code` (`DN00InteropResponseStatus`), `errorId` and `details`. See [Status](../semantica-base/modelo/status.md)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    }
  },
  "code": "OK",
  "payload": {
    "dataItems": [
      {
        "data": {
          "type": "administrativeServiceProcedureRecord",
          "oid": "EXP-OID-001",
          "id": "EXP-2024-00123",
          "service": {
            "serviceNameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
            "originRef": { "id": "SRV-LIC-ACT" }
          },
          "procedure": {
            "serviceNameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
            "originRef": { "id": "PROC-LIC-APER" }
          },
          "createdAt": "2024-03-15T10:30:00Z",
          "lastUpdatedAt": "2024-06-01T14:00:00Z",
          "applicationDate": "2024-03-14T09:00:00Z",
          "regNumber": "REG-2024-00123",
          "interested": { "partyId": "12345678A", "partyName": "Juan García" },
          "state": {
            "stateCode": "IN_PROGRESS",
            "description": { "SPANISH": "En tramitación", "BASQUE": "Izapidetzen" }
          },
          "urls": [
            { "url": "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language": "SPANISH", "tags": ["default"] }
          ]
        },
        "proposedScheduleItems": []
      }
    ],
    "itemsPagingContext": null
  }
}
```

> **Response structure** (`DN00DataRetrieveResponseFromAdmin`): the `code` (status) is at root level, sibling of `context` and `payload`. Inside `payload`, `dataItems` is an array where **each element wraps the business object in a `data` field** (`DN00DataRetrievedFromAdmin`), with an optional `proposedScheduleItems` (appointments the administration proposes to show on the client). `itemsPagingContext` is optional (paging).

## Response with no data (HTTP 200)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    }
  },
  "code": "OK",
  "payload": { "dataItems": [] }
}
```

## Error response (HTTP 4xx/5xx)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    }
  },
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Persona no encontrada en el sistema" }
}
```

### Status codes (`code`)

| Code | Description |
|--------|-------------|
| `OK` | Message processed successfully |
| `CLIENT_ERR` | Client error (malformed request, person not found) |
| `SERVER_ERR` | Server error (internal error) |
| `QUEUED` | Message queued for asynchronous processing |

---

## Object types in `dataItems[].data`

Each element in the `dataItems` array wraps the business object in its `data` field. That object inherits the [common fields](./data/campos-comunes.md) (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) and adds specific fields depending on its type:

| `type` | Object | Documentation |
|--------|--------|---------------|
| `administrativeServiceProcedureRecord` | Record | [expediente.md](./data/expediente.md) |
| `administrativeNotice` | Notification | [notificacion.md](./data/notificacion.md) |
| `administrativeOfficialRegisterRecord` | Official registry | [registro-oficial.md](./data/registro-oficial.md) |
| `oneOffPayment` | One-off payment | [pago.md](./data/pago.md) |
| `directDebitPayment` | Direct debit | [pago.md](./data/pago.md) |
| `scheduleItem` | Appointment | [cita.md](./data/cita.md) |

Referenced auxiliary objects:

| Object | Documentation |
|--------|---------------|
| Service / Procedure | [servicio-administrativo.md](./data/servicio-administrativo.md) |
| Organizational Unit | [unidad-organica.md](./data/unidad-organica.md) |
| Common fields (base) | [campos-comunes.md](./data/campos-comunes.md) |

---

## Example — Notification

```json
{
  "type": "OFFICIAL_NOTICE",
  "oid": "NOT-OID-001",
  "id": "NOT-2024-00456",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "issuedAt": "2024-05-20T09:00:00Z",
  "readedAt": null,
  "state": "PENDING_TO_BE_READED_BY_DESTINATION",
  "actSubjectByLanguage": { "SPANISH": "Resolución de concesión de ayuda", "BASQUE": "Laguntza emateko ebazpena" },
  "urls": [{ "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00456", "language": "SPANISH", "tags": ["default"] }]
}
```

## Example — One-off payment

```json
{
  "type": "oneOffPayment",
  "oid": "PAY-OID-001",
  "id": "PAY-2024-00321",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "paymentType": "ONE_OFF_PAYMENT",
  "paymentSubjectByLanguage": { "SPANISH": "Tasa por licencia de actividad", "BASQUE": "Jarduera-lizentziaren tasa" },
  "paymentDates": { "dueDate": "2024-06-30", "surchargedAt": "2024-07-15", "paidAt": null },
  "format": "502",
  "amount": { "amount": 45.50, "currency": "EUR" },
  "amountIfSurcharged": { "amount": 50.05, "currency": "EUR" },
  "data": { "forStatus": "PENDING", "at": null, "medium": null, "device": null },
  "urls": [{ "url": "https://pago.miadmin.eus/pay/PAY-2024-00321", "language": "SPANISH", "tags": ["payment"] }]
}
```

## Example — Official registry

```json
{
  "type": "administrativeOfficialRegisterRecord",
  "oid": "REG-OID-001",
  "id": "REG-2024-00789",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "registeredAt": "2024-04-10T08:30:00Z",
  "subjectByLanguage": { "SPANISH": "Solicitud de licencia de obras", "BASQUE": "Obra-lizentzia eskaera" },
  "state": { "stateCode": "PRESENTED", "description": { "SPANISH": "Presentado", "BASQUE": "Aurkeztua" } }
}
```

## Example — Appointment

```json
{
  "type": "scheduleItem",
  "oid": "SCHED-OID-001",
  "id": "CITA-2024-00050",
  "year": 2024,
  "monthOfYear": 7,
  "dayOfMonth": 15,
  "hourOfDay": 10,
  "minuteOfHour": 30,
  "durationMinutes": 30,
  "priority": "NORMAL",
  "subject": { "SPANISH": "Cita previa para renovación de DNI", "BASQUE": "NAN berritzeko aurretiko hitzordua" },
  "location": {
    "country": { "id": "ES", "name": "España" },
    "administrativeAreaLevel1": { "id": "PV", "name": "País Vasco" },
    "administrativeAreaLevel3": { "id": "48020", "name": "Bilbao" },
    "zipCode": "48001",
    "address": "Gran Vía 50, Bilbao"
  }
}
```

## Example — Direct debit

```json
{
  "type": "directDebitPayment",
  "oid": "DD-OID-001",
  "id": "DD-2024-00100",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "paymentType": "DIRECT_DEBIT",
  "paymentSubjectByLanguage": { "SPANISH": "Cuota mensual guardería", "BASQUE": "Haur-eskolako hileko kuota" },
  "directDebitData": {
    "startDate": "2024-01-15",
    "expiresAt": null,
    "frequency": "MONTHLY",
    "medium": "DIRECT_DEBIT",
    "mediumHint": "2100 ***** 051332"
  },
  "nextChargeAt": "2024-07-01",
  "nextChargeAmountInEuro": 120.00,
  "paymentStatus": "ACTIVE",
  "history": [
    { "at": "2024-06-01", "amountInEuro": 120.00 }
  ]
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
|--------|-------------|
| `200` | Data returned successfully (can be an empty list) |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorized (invalid or expired token) |
| `403` | Forbidden (no permissions) |
| `404` | Person not found |
| `500` | Internal error |
| `503` | Service unavailable |

---

## Requirements for the administration

1. Expose a `POST` endpoint that accepts and returns `application/json` at the URL you configured in DENA (the reference base connector publishes it at `/api/connector/retrieveData`)
2. Interpret `context.subjectPerson.id` to identify the person
3. Interpret `context.dataType.id` to filter the data type
4. Return objects in the semantic model format
5. Include multilingual texts (Spanish and Basque as a minimum)
6. Include URLs to the electronic office when possible
7. Respect standard HTTP codes
8. Return HTTP 200 with `dataItems: []` when there is no data (do not use 404)
9. Respond in less than 30 seconds
10. Use `code: "OK"` in successful responses and `code: "CLIENT_ERR"` or `code: "SERVER_ERR"` in errors

---

## Related documentation

| Document | Content |
|-----------|----------|
| [campos-comunes.md](./data/campos-comunes.md) | Fields inherited by all objects (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) |
| [expediente.md](./data/expediente.md) | Administrative record |
| [notificacion.md](./data/notificacion.md) | Notification / communication |
| [registro-oficial.md](./data/registro-oficial.md) | Registry entry |
| [pago.md](./data/pago.md) | One-off payment and direct debit |
| [cita.md](./data/cita.md) | Appointment / schedule item |
| [servicio-administrativo.md](./data/servicio-administrativo.md) | Service and procedure |
| [unidad-organica.md](./data/unidad-organica.md) | Organizational unit |
| [validaciones.md](./validaciones.md) | Format and validation rules |
| [errores-troubleshooting.md](./errores-troubleshooting.md) | Common errors guide |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
