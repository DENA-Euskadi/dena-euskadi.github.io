# Endpoint DATA-RETRIEVE — Especificación para Administraciones

## Endpoint

```
POST /api/retrieveData
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

---

## Request

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "RECORDS" },
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
  "protocol": {
    "urls": [],
    "timeOut": "30s"
  },
  "data": {
    "person": "PERSON-OID-001"
  }
}
```

| Campo | Obligatorio | Descripción |
|-------|:-----------:|-------------|
| `context.messageType` | ✅ | Tipo de mensaje (`PERSON_FETCH_DATA`) |
| `context.dataType.dataTypeId` | ✅ | Tipo de dato: `RECORDS`, `NOTICES`, `REGISTRY`, `PAYMENTS`, `SCHEDULE` |
| `context.messageCorrelationId` | ✅ | UUID de correlación para trazabilidad |
| `context.flowDirection` | ✅ | Dirección del flujo: `REQUEST` |
| `context.originPartyId` | ❌ | Identificador del origen (ej: `DENA-CORE`) |
| `context.destinationPartyId` | ❌ | Identificador del destino (ej: `ADMIN-001`) |
| `context.subjectPerson.personId` | ✅ | DNI/NIE/NIF de la persona |
| `context.administration.administrationId` | ❌ | Identificador de la administración |
| `context.administration.dir3Code` | ❌ | Código DIR3 |
| `context.interopRouteData` | ❌ | Traza de componentes DENA |
| `consentOid` | ❌ | OID del consentimiento otorgado |
| `protocol.urls` | ❌ | URLs de plantilla del protocolo |
| `protocol.timeOut` | ❌ | Timeout (ej: `"30s"`) |
| `data` | ✅ | Payload de la petición |

---

## Response exitosa (HTTP 200)

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "RECORDS" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" },
    "administration": { "administrationId": "ADMIN-001" }
  },
  "data": {
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

## Response sin datos (HTTP 200)

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "RECORDS" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": { "dataItems": [] },
  "code": "OK"
}
```

## Response de error (HTTP 4xx/5xx)

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
  "details": { "details": "Persona no encontrada en el sistema" }
}
```

### Códigos de estado (`code`)

| Código | Descripción |
|--------|-------------|
| `OK` | Mensaje procesado correctamente |
| `CLIENT_ERR` | Error del cliente (petición malformada, persona no encontrada) |
| `SERVER_ERR` | Error del servidor (error interno) |
| `QUEUED` | Mensaje encolado para procesamiento asíncrono |

---

## Tipos de objeto en `dataItems`

| `type` | Objeto | Documentación |
|--------|--------|---------------|
| `administrativeServiceProcedureRecord` | Expediente | [expediente.md](./data/expediente.md) |
| `administrativeNotice` | Notificación | [notificacion.md](./data/notificacion.md) |
| `administrativeOfficialRegistryRecord` | Registro oficial | [registro-oficial.md](./data/registro-oficial.md) |
| `oneOffPayment` | Pago único | [pago.md](./data/pago.md) |
| `directDebitPayment` | Domiciliación | [pago.md](./data/pago.md) |
| `scheduleItem` | Cita | [cita.md](./data/cita.md) |

---

## Ejemplo — Notificación

```json
{
  "type": "administrativeNotice",
  "oid": "NOT-OID-001",
  "id": "NOT-2024-00456",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "noticeType": "OFFICIAL_NOTICE",
  "issuedAt": "2024-05-20T09:00:00Z",
  "readedAt": null,
  "state": "PENDING_TO_BE_READED_BY_DESTINATION",
  "actSubjectByLanguage": { "SPANISH": "Resolución de concesión de ayuda", "BASQUE": "Laguntza emateko ebazpena" },
  "urls": [{ "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00456", "language": "SPANISH", "tags": ["default"] }]
}
```

## Ejemplo — Pago único

```json
{
  "type": "oneOffPayment",
  "oid": "PAY-OID-001",
  "id": "PAY-2024-00321",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "paymentType": "ONE_OFF_PAYMENT",
  "paymentSubjectByLanguage": { "SPANISH": "Tasa por licencia de actividad", "BASQUE": "Jarduera-lizentziaren tasa" },
  "paymentDates": { "dueDate": "2024-06-30", "surchargedAt": "2024-07-15", "paidAt": null },
  "amountInEuro": 45.50,
  "amountInEuroIfSurcharged": 50.05,
  "data": { "forStatus": "PENDING", "at": null, "medium": null, "device": null },
  "urls": [{ "url": "https://pago.miadmin.eus/pay/PAY-2024-00321", "language": "SPANISH", "tags": ["payment"] }]
}
```

## Ejemplo — Registro oficial

```json
{
  "type": "administrativeOfficialRegistryRecord",
  "oid": "REG-OID-001",
  "id": "REG-2024-00789",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "registeredAt": "2024-04-10T08:30:00Z",
  "subjectByLanguage": { "SPANISH": "Solicitud de licencia de obras", "BASQUE": "Obra-lizentzia eskaera" },
  "state": { "stateCode": "PRESENTED", "description": { "SPANISH": "Presentado", "BASQUE": "Aurkeztua" } }
}
```

## Ejemplo — Cita

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

## Ejemplo — Domiciliación

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
  "history": [
    { "at": "2024-06-01", "amountInEuro": 120.00 }
  ]
}
```

---

## Autenticación

Si la administración requiere OAuth2, recibirá la cabecera:

```
Authorization: Bearer <access_token>
```

El token se obtiene automáticamente mediante client credentials.

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Datos devueltos correctamente (puede ser lista vacía) |
| `400` | Petición malformada o parámetros inválidos |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `404` | Persona no encontrada |
| `500` | Error interno |
| `503` | Servicio no disponible |

---

## Requisitos para la administración

1. Exponer un endpoint `POST` que acepte y devuelva `application/json`
2. Interpretar `context.subjectPerson.personId` para identificar a la persona
3. Interpretar `context.dataType.dataTypeId` para filtrar el tipo de datos
4. Devolver los objetos en el formato del modelo semántico
5. Incluir textos multiidioma (castellano y euskera como mínimo)
6. Incluir URLs de acceso a la sede electrónica cuando sea posible
7. Respetar los códigos HTTP estándar
8. Devolver HTTP 200 con `dataItems: []` cuando no hay datos (no usar 404)
9. Responder en menos de 30 segundos
10. Usar `code: "OK"` en respuestas exitosas y `code: "CLIENT_ERR"` o `code: "SERVER_ERR"` en errores

---

## Documentación relacionada

- [campos-comunes.md](./data/campos-comunes.md) — Campos heredados por todos los objetos
- [validaciones.md](./validaciones.md) — Reglas de formato y validación
- [errores-troubleshooting.md](./errores-troubleshooting.md) — Guía de errores comunes
