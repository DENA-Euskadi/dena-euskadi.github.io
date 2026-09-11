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

El cuerpo de la petición es un objeto `DN00PersonSyncPushToAdminFromCOREToConnectorInternalSide` (`@MarshallType(as="personSyncPushToAdminFromCOREToConnectorInternalSide")`), con la configuración de origen y la **notificación** que contiene los datos de la persona:

```json
{
  "dataOriginConfigForDataTypeInAdmin": { "...": "configuración interna del origen de datos (uso del conector)" },
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
      "contactInfo": { "...": "datos de contacto (ContactInfo)" },
      "lastChangeEvent": "CREATED"
    }
  }
}
```

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `dataOriginConfigForDataTypeInAdmin` | `Object` | ✅ | Configuración del origen de datos para el tipo de dato en la administración. Es información interna que usa el conector; la administración no necesita interpretarla |
| `notification` | `DN00PersonSyncPushToAdminNotification` (`@MarshallType(as="personSyncPushToAdminNotification")`) | ✅ | Notificación con los datos de la persona a sincronizar |

## `notification`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `syncData` | `DN00PersonSyncData` (`@MarshallType(as="personSyncData")`) | ✅ | Metadatos de la sincronización (referencia, hashes, fechas, evento) |
| `person` | `DN00Person` (`@MarshallType(as="person")`) | ✅ | Datos completos de la persona |

### `notification.syncData`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `personRef` | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Referencia a la persona creada o modificada (`oid`/`id`) |
| `personHashes` | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Hashes de nombre y apellidos para su identificación inequívoca |
| `createDate` | `Instant` (ISO 8601) | ❌ | Fecha de creación |
| `lastUpdateDate` | `Instant` (ISO 8601) | ❌ | Fecha de última actualización |
| `syncEvent` | `DN00PersonChangeEvent` | ❌ | Evento que disparó la sincronización: `CREATED` (nueva persona), `DELETED` (persona eliminada), `UPDATED` (datos actualizados), `ID_CHANGED` (identificador modificado) |

### `notification.person`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `oid` / `id` | `String` | ✅ | Identificadores de la persona |
| `name` | `String` | ✅ | Nombre |
| `surname1` | `String` | ✅ | Primer apellido |
| `surname2` | `String` | ❌ | Segundo apellido |
| `contactInfo` | `ContactInfo` | ❌ | Datos de contacto |
| `lastChangeEvent` | `DN00PersonChangeEvent` | ❌ | Último tipo de cambio aplicado (lo fija DENA-CORE) |

---

## Response

La administración indica el resultado del procesamiento mediante el **código de estado HTTP**:

- **`200 OK`** — la notificación se procesó correctamente. No es obligatorio devolver cuerpo.
- **`4xx`** — error atribuible a la petición (p. ej. `404` si la persona no se puede resolver, `400` si el cuerpo es inválido).
- **`5xx`** — error interno de la administración.

DENA-CORE interpreta el resultado a partir del código HTTP (ver `DN01PersonPushToAdminJobProcessor`): si la respuesta es satisfactoria el job pasa a `SYNCED_OK`; en caso contrario se reintenta (hasta el máximo de intentos) y pasa a `SYNCED_ERROR` / `SYNCED_ERROR_TOO_MANY_ATTEMPTS`.

Si la administración devuelve un cuerpo de error, se recomienda un objeto simple con un mensaje descriptivo, por ejemplo:

```json
{
  "error": "PERSON_NOT_FOUND",
  "message": "Persona no encontrada en el sistema"
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
2. Interpretar `notification.syncData.personRef` (y `notification.person`) para identificar a la persona
3. Actualizar su base de datos de personas registradas en DENA con la información recibida
4. Respetar los códigos HTTP estándar (`200` si se procesa correctamente; `4xx`/`5xx` en error)
5. Responder en menos de 30 segundos

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
