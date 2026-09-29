# Endpoint DATA-RETRIEVE — Especificación para Administraciones

## Endpoint

```
POST /api/retrieveData
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

!!! note "La ruta la eliges tú"

    DENA **no impone** un path fijo: hará el `POST` contra la **URL que tu administración haya configurado** en DENA. `/api/retrieveData` es solo un ejemplo (el que usa esta documentación). El **conector base** de DENA (`DN01ConnectorController`) lo expone en `/api/connector/retrieveData`, que puedes tomar como referencia. Lo imprescindible es aceptar un `POST` con `application/json`.

---

## Request

La administración recibe del conector DENA una petición con un objeto `context` (persona, tipo de dato y administración) y un `payload` con la petición de recuperación. Estos son los campos relevantes que debe leer la administración:

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

| Campo | Obligatorio | Descripción |
|-------|:-----------:|-------------|
| `context.subjectPerson.id` | ✅ | DNI/NIE/NIF de la persona cuyos datos se solicitan |
| `context.dataType.id` | ✅ | Tipo de dato solicitado (marshallTypeId): `administrativeServiceProcedureRecord`, `administrativeNotice`, `administrativeOfficialRegisterRecord`, `oneOffPayment`, `directDebitPayment`, `scheduleItem`, `personData`. Ver [DataTypeRef](../semantica-base/modelo/data-type-ref.md) y [`DN00DataTypeEnum`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DataTypeEnum.java) |
| `context.destinationAdmin.id` | ✅ | Identificador de la administración de destino (la que sirve los datos) |
| `context.originAdmin.id` | ❌ | Identificador del origen de la petición (el conector DENA) |

!!! info "Nombres de los campos"

    Dentro de `context` los campos se llaman `subjectPerson`, `dataType` y `destinationAdmin` (**no** `administration`). El `payload` interno repite la petición como `person` / `admin` / `dataType`. Para implementar el endpoint basta con leer `context.subjectPerson.id` y `context.dataType.id` (y `context.destinationAdmin.id` si sirves varias administraciones).

---

## Response exitosa (HTTP 200)

> Estado de la respuesta: `code` (`DN00InteropResponseStatus`), `errorId` y `details`. Ver [Status](../semantica-base/modelo/status.md)

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

> **Estructura de la respuesta** (`DN00DataRetrieveResponseFromAdmin`): el `code` (estado) va a nivel raíz, hermano de `context` y `payload`. Dentro de `payload`, `dataItems` es un array donde **cada elemento envuelve el objeto de negocio en un campo `data`** (`DN00DataRetrievedFromAdmin`), con un `proposedScheduleItems` opcional (citas que la administración propone mostrar al cliente). `itemsPagingContext` es opcional (paginación).

## Response sin datos (HTTP 200)

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

## Response de error (HTTP 4xx/5xx)

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

### Códigos de estado (`code`)

| Código | Descripción |
|--------|-------------|
| `OK` | Mensaje procesado correctamente |
| `CLIENT_ERR` | Error del cliente (petición malformada, persona no encontrada) |
| `SERVER_ERR` | Error del servidor (error interno) |
| `QUEUED` | Mensaje encolado para procesamiento asíncrono |

---

## Tipos de objeto en `dataItems[].data`

Cada elemento del array `dataItems` envuelve el objeto de negocio en su campo `data`. Ese objeto hereda los [campos comunes](./data/campos-comunes.md) (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) y añade campos específicos según su tipo:

| `type` | Objeto | Documentación |
|--------|--------|---------------|
| `administrativeServiceProcedureRecord` | Expediente | [expediente.md](./data/expediente.md) |
| `administrativeNotice` | Notificación | [notificacion.md](./data/notificacion.md) |
| `administrativeOfficialRegisterRecord` | Registro oficial | [registro-oficial.md](./data/registro-oficial.md) |
| `oneOffPayment` | Pago único | [pago.md](./data/pago.md) |
| `directDebitPayment` | Domiciliación | [pago.md](./data/pago.md) |
| `scheduleItem` | Cita | [cita.md](./data/cita.md) |

Objetos auxiliares referenciados:

| Objeto | Documentación |
|--------|---------------|
| Servicio / Procedimiento | [servicio-administrativo.md](./data/servicio-administrativo.md) |
| Unidad Orgánica | [unidad-organica.md](./data/unidad-organica.md) |
| Campos comunes (base) | [campos-comunes.md](./data/campos-comunes.md) |

---

## Ejemplo — Notificación

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
  "format": "502",
  "amount": { "amount": 45.50, "currency": "EUR" },
  "amountIfSurcharged": { "amount": 50.05, "currency": "EUR" },
  "data": { "forStatus": "PENDING", "at": null, "medium": null, "device": null },
  "urls": [{ "url": "https://pago.miadmin.eus/pay/PAY-2024-00321", "language": "SPANISH", "tags": ["payment"] }]
}
```

## Ejemplo — Registro oficial

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
  "paymentStatus": "ACTIVE",
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

1. Exponer un endpoint `POST` que acepte y devuelva `application/json` en la URL que hayas configurado en DENA (el conector base de referencia lo publica en `/api/connector/retrieveData`)
2. Interpretar `context.subjectPerson.id` para identificar a la persona
3. Interpretar `context.dataType.id` para filtrar el tipo de datos
4. Devolver los objetos en el formato del modelo semántico
5. Incluir textos multiidioma (castellano y euskera como mínimo)
6. Incluir URLs de acceso a la sede electrónica cuando sea posible
7. Respetar los códigos HTTP estándar
8. Devolver HTTP 200 con `dataItems: []` cuando no hay datos (no usar 404)
9. Responder en menos de 30 segundos
10. Usar `code: "OK"` en respuestas exitosas y `code: "CLIENT_ERR"` o `code: "SERVER_ERR"` en errores

---

## Documentación relacionada

| Documento | Contenido |
|-----------|----------|
| [campos-comunes.md](./data/campos-comunes.md) | Campos heredados por todos los objetos (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) |
| [expediente.md](./data/expediente.md) | Expediente administrativo |
| [notificacion.md](./data/notificacion.md) | Notificación / comunicación |
| [registro-oficial.md](./data/registro-oficial.md) | Asiento registral |
| [pago.md](./data/pago.md) | Pago único y domiciliación |
| [cita.md](./data/cita.md) | Cita previa / elemento de agenda |
| [servicio-administrativo.md](./data/servicio-administrativo.md) | Servicio y procedimiento |
| [unidad-organica.md](./data/unidad-organica.md) | Unidad organizativa |
| [validaciones.md](./validaciones.md) | Reglas de formato y validación |
| [errores-troubleshooting.md](./errores-troubleshooting.md) | Guía de errores comunes |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
