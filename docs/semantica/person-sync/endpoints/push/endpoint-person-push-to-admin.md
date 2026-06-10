# Endpoint Person Push To Admin — Especificación para Administraciones

## Endpoint

```
POST /api/person/push
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
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

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md) | ✅          | Objeto de contexto de la petición |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |

## Data

| Campo            | Tipo     | Obligatorio | Descripción |
|------------------|----------|-------------|-------------|
| `personRef`      | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Referencia a la persona creada o modificada |
| `personHashes`   | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Hashes de nombre y apellidos de la persona para su identificación inequivoca |
| `createDate`     | `ISO 8601 Date` | ✅ | Fecha de creación |
| `lastUpdateDate` | `ISO 8601 Date` | ❌ | Fecha de ultima actualización |
| `syncEvent`      | `String` | ✅ | Evento producido. Valores posibles: <br> `CREATED`: Nueva persona registrada <br> `DELETED`: Persona eliminada de DENA <br> `UPDATED`: Datos de la persona actualizados <br> `ID_CHANGED`: Identificador de la persona modificado |

---

## Response exitosa (HTTP 200)

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

## Response de error (HTTP 4xx/5xx)

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
2. Interpretar `data.personRef` para identificar a la persona
3. Actualizar su base de datos de personas registradas en DENA con la información recibida
4. Respetar los códigos HTTP estándar
5. Responder en menos de 30 segundos
6. Usar `code: "OK"` en respuestas exitosas y `code: "CLIENT_ERR"` o `code: "SERVER_ERR"` en errores

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
