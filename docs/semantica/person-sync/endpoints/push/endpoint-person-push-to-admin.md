# Endpoint Person Push To Admin — Especificación para Administraciones

Este documento describe **qué debe implementar tu administración** para recibir las notificaciones de cambios de personas que DENA envía de forma proactiva (mecanismo *Push*).

---

## ¿Quién llama a quién?

En el Push, **DENA-CORE actúa como cliente HTTP** y tu administración como servidor:

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (cliente)
    participant Admin as Tu administración (servidor)

    Note over DENA: Se registra / cambia / elimina una persona
    DENA->>Admin: POST <tu-url-configurada> (body JSON con el cambio)
    Admin->>Admin: Procesa el cambio (alta / actualización / borrado)
    Admin-->>DENA: 200 OK
```

!!! important "No hay una ruta fija predefinida"

    DENA **no impone** un path concreto para tu endpoint. Tu administración expone el endpoint en **la URL que hayas configurado** en DENA para tu conector (o para el acceso directo, en entornos de desarrollo). DENA hará el `POST` contra **esa** URL.

    Lo único que DENA exige es que la URL acepte un `POST` con `Content-Type: application/json` y responda con el código de estado HTTP adecuado.

---

## Petición (Request)

DENA envía un `POST` cuyo cuerpo es un objeto `DN00PersonSyncPushToAdminFromCOREToConnectorInternalSide` (`@MarshallType(as="personSyncPushToAdminFromCOREToConnectorInternalSide")`), con la configuración de origen y la **notificación** que contiene los datos de la persona:

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
| `dataOriginConfigForDataTypeInAdmin` | `Object` | ✅ | Configuración del origen de datos para el tipo de dato en la administración. Es información interna que usa el conector; **tu administración no necesita interpretarla** |
| `notification` | `DN00PersonSyncPushToAdminNotification` (`@MarshallType(as="personSyncPushToAdminNotification")`) | ✅ | Notificación con los datos de la persona a sincronizar. **Es lo que tu administración debe procesar** |

### `notification`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `syncData` | `DN00PersonSyncData` (`@MarshallType(as="personSyncData")`) | ✅ | Metadatos de la sincronización (referencia a la persona, hashes, fechas y **evento**) |
| `person` | `DN00Person` (`@MarshallType(as="person")`) | ✅ | Datos completos de la persona |

### `notification.syncData`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `personRef` | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Referencia a la persona creada/modificada/eliminada (`oid` y/o `id`). **Es la clave que debes usar para localizar a la persona en tu sistema** |
| `personHashes` | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Hashes de nombre y apellidos, para identificación inequívoca sin exponer los datos en claro |
| `createDate` | `Instant` (ISO 8601) | ❌ | Fecha de creación de la persona en DENA |
| `lastUpdateDate` | `Instant` (ISO 8601) | ❌ | Fecha de última actualización |
| `syncEvent` | `DN00PersonChangeEvent` | ✅ | **Qué cambio ha ocurrido**. Determina la acción que debe realizar tu administración (ver [Procesamiento por evento](#procesamiento-por-evento)). Valores: `CREATED`, `UPDATED`, `DELETED`, `ID_CHANGED` |

### `notification.person`

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `oid` | `String` | ✅ | Identificador único de la persona generado por DENA (estable, no cambia) |
| `id` | `String` | ✅ | NIF/NIE de la persona (puede cambiar → ver evento `ID_CHANGED`) |
| `name` | `String` | ✅ | Nombre |
| `surname1` | `String` | ✅ | Primer apellido |
| `surname2` | `String` | ❌ | Segundo apellido |
| `contactInfo` | `ContactInfo` | ❌ | Datos de contacto |
| `lastChangeEvent` | `DN00PersonChangeEvent` | ❌ | Último tipo de cambio aplicado (lo fija DENA-CORE) |

!!! tip "OID vs ID: cuál usar como clave"

    Guarda a las personas por su **`oid`** (identificador estable de DENA), no por el `id` (NIF). El NIF puede cambiar (por ejemplo un NIE que pasa a DNI) y en ese caso recibirás un evento `ID_CHANGED`. Si indexas por `oid`, esos cambios de NIF son una simple actualización.

---

## Procesamiento por evento

El campo `notification.syncData.syncEvent` te dice **qué acción realizar**. Esta es la implementación esperada de tu administración para cada evento:

| Evento | Significado | Qué debe hacer tu administración |
|--------|-------------|----------------------------------|
| `CREATED` | Una persona nueva se ha registrado en DENA | Dar de alta a la persona en tu copia local (o *upsert* si ya existiera) con los datos recibidos |
| `UPDATED` | La persona ha cambiado datos básicos (nombre, contacto...) | Actualizar los datos de la persona en tu copia local |
| `ID_CHANGED` | El identificador (NIF/NIE) de la persona ha cambiado | Actualizar el `id` de la persona (localizándola por su `oid`, que no cambia) |
| `DELETED` | La persona ha eliminado su cuenta en DENA | Borrar a la persona de tu copia local **y también todos los datos asociados** a ella |

!!! warning "Evento DELETED: borra también los datos asociados"

    Cuando recibas un `DELETED`, no basta con borrar el registro de la persona. Debes eliminar **también los datos que tu administración tuviera asociados a esa persona** (por ejemplo, avisos/expedientes generados, entradas de log de sincronización, etc.).

    Si solo borras a la persona y dejas sus datos, quedarán **datos huérfanos** (filas que referencian a una persona que ya no existe). Esto provoca inconsistencias y puede hacer que envíes SRMD de una persona que ya no está en DENA.

    Recomendación: borra primero los datos asociados (por el identificador de persona) y después el registro de la persona.

---

## Respuesta (Response)

Tu administración indica el resultado **exclusivamente mediante el código de estado HTTP**:

- **`200 OK`** — la notificación se procesó correctamente. **No es necesario devolver cuerpo.**
- **`4xx`** — error atribuible a la petición (p. ej. `400` si el cuerpo es inválido).
- **`5xx`** — error interno de tu administración.

!!! info "DENA solo lee el código HTTP, no el cuerpo de la respuesta"

    DENA-CORE (`DN01PersonPushToAdminJobProcessor`) interpreta el resultado **únicamente a partir del código HTTP**. El cuerpo de la respuesta **no se procesa** (a lo sumo se registra en el log para diagnóstico).

    - Si la respuesta es satisfactoria (`2xx`), el job de push pasa a `SYNCED_OK`.
    - Si no lo es, DENA **reintenta** (hasta un máximo de intentos) y, si sigue fallando, el job pasa a `SYNCED_ERROR` / `SYNCED_ERROR_TOO_MANY_ATTEMPTS`.

    Por tanto, es fundamental que devuelvas un código HTTP **fiel** al resultado real: no devuelvas `200` si el procesamiento falló, o DENA dará por sincronizada una persona que no lo está.

Si aun así quieres devolver un cuerpo de error para facilitar el diagnóstico (opcional, DENA no lo interpreta), puedes usar un objeto simple:

```json
{
  "error": "PERSON_NOT_FOUND",
  "message": "Persona no encontrada en el sistema"
}
```

---

## Autenticación

Si tu administración requiere OAuth2, DENA incluirá la cabecera:

```
Authorization: Bearer <access_token>
```

El token lo obtiene DENA automáticamente mediante *client credentials*. Consulta la sección de [Autenticación](../../../../autenticacion/core-dena-administracion/index.md) para configurarlo.

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Notificación procesada correctamente |
| `400` | Petición malformada o parámetros inválidos |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `404` | Persona no encontrada / no resoluble |
| `500` | Error interno de la administración |
| `503` | Servicio no disponible |

---

## Checklist de implementación para la administración

1. **Exponer un endpoint `POST`** en la URL que hayas configurado en DENA, que acepte `application/json`.
2. **Leer `notification.syncData.syncEvent`** para decidir la acción (alta / actualización / borrado).
3. **Localizar a la persona por `notification.syncData.personRef.oid`** (identificador estable).
4. **Aplicar el cambio** según el evento (ver [Procesamiento por evento](#procesamiento-por-evento)), recordando borrar los datos asociados en `DELETED`.
5. **Responder con el código HTTP fiel** al resultado (`200` solo si se procesó bien; `4xx`/`5xx` en error).
6. **Responder en menos de 30 segundos** (si no, DENA considera la llamada fallida y reintentará).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
