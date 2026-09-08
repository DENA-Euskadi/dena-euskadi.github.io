# Endpoint DATA-RETRIEVE — Specification for Administrations

## Endpoint

```
POST /api/retrieveData
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

---

## Request

> Context Java class: [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": [
        { "denaComponentId": "apiGateway", "timestamp": "2024-06-01T10:00:00Z" }
      ]
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DATATYPE-OID-001" },
    "originClientInstallment": "CLIENT-INSTALLMENT-OID-001",
    "destinationAdmin": { "oid": "ADMIN-OID-001", "id": "ADMIN-001", "dir3Id": "EA0000001" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
  },
  "protocol": {
    "urls": [],
    "timeOut": "30s"
  },
  "payload": {
    "person": "PERSON-OID-001"
  }
}
```

| Field | Mandatory | Description |
|-------|:-----------:|-------------|
| `context.message.type` | ✅ | Message type (`PERSON_FETCH_DATA`). See [`DN00InteropMessageType`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageType.java) |
| `context.message.correlationId` | ✅ | Correlation UUID for traceability. See [`DN00InteropMessageData`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageData.java) |
| `context.message.interopRouteData` | ❌ | DENA component trace. See [`DN00IteropRouteDataItem`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00IteropRouteDataItem.java) |
| `context.dataType.id` | ✅ | Data type (marshallTypeId): `administrativeServiceProcedureRecord`, `administrativeNotice`, `administrativeOfficialRegisterRecord`, `oneOffPayment`, `directDebitPayment`, `scheduleItem`, `personData`. See [`DN00DataTypeEnum`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DataTypeEnum.java) |
| `context.dataType.oid` | ❌ | Data type OID (DataTypeRef). See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.originClientInstallment` | ❌ | OID of the origin client installment (when the message is sent by a client device). See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin.oid` | ❌ | Destination administration OID (OrgAdminRef). See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin.id` | ❌ | Destination administration identifier. See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin.dir3Id` | ❌ | Destination administration DIR3 code. See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.subjectPerson.id` | ✅ | DNI/NIE/NIF of the person (PersonRef). See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.subjectPerson.oid` | ❌ | Person OID. See [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `protocol.urls` | ❌ | Protocol template URLs. See [`DN00InteropProtocol`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropProtocol.java) |
| `protocol.timeOut` | ❌ | Timeout (e.g.: `"30s"`). See [`DN00InteropProtocol`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropProtocol.java) |
| `payload` | ✅ | Request payload |

> The flow direction (`REQUEST`/`RESPONSE`) is a value derived from the message type and is not serialized in the JSON. See [`DN00InteropFlowDirection`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropFlowDirection.java)

---

## Successful response (HTTP 200)

> Response status: `code` (`DN00InteropResponseStatus`), `errorId` and `details`. See [Status](../semantica-base/modelo/status.md)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DATATYPE-OID-001" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" },
    "destinationAdmin": { "oid": "ADMIN-OID-001", "id": "ADMIN-001" }
  },
  "payload": {
    "dataItems": [
      {
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
      }
    ]
  },
  "code": "OK",
  "errorId": null,
  "details": null
}
```

## Response with no data (HTTP 200)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DATATYPE-OID-001" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
  },
  "payload": { "dataItems": [] },
  "code": "OK"
}
```

## Error response (HTTP 4xx/5xx)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
  },
  "payload": null,
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

## Object types in `dataItems`

Each element in the `dataItems` array is an object that inherits the [common fields](./data/campos-comunes.md) (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) and adds specific fields depending on its type:

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

1. Expose a `POST` endpoint that accepts and returns `application/json`
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
