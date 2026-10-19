# PERSON-SYNC — Consulta de Persona (Fetch)

## Endpoint

Dos variantes según identifiques a la persona por su **OID** (identificador estable de DENA) o por su **ID** (NIF/NIE):

```
POST /person/api/admin/persons/{oid}
POST /person/api/admin/persons/byId/{id}
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

## Descripción

Recupera los **datos completos** de una persona registrada en DENA. Forma parte de la **Consulta de Personas** (consulta REST en tiempo real) del servicio `person-admin-rest`. Usa la variante por `oid` siempre que puedas (es estable); usa la variante por `id` cuando solo dispongas del NIF/NIE.

## Request

```json
{
    "context": {
        "message": {
            "type": "PERSON_FETCH_DATA",
            "correlationId": "c5e2726b-225e-4958-90fe-7cb9291bdbfa",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-05-16T10:15:30Z"
                }
            ]
        },
        "originAdmin": {
            "oid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011"
        }
    },
    "payload": {
        "oid": "96C615E2-B24E-4449-B6B4-9551A4EE0A9D"
    }
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)    | ✅          | Objeto de contexto de la petición, con `message.type` con valor `PERSON_FETCH_DATA` |
| `payload` | [Payload](#payload)                            | ✅          | Referencia a la persona a recuperar |

## Payload

| Campo | Tipo     | Obligatorio | Descripción |
|-------|----------|-------------|-------------|
| `oid` | `String` | ✅ (variante por OID) | OID de la persona en DENA |
| `id`  | `String` | ✅ (variante por ID)  | NIF/NIE de la persona |

!!! note "Qué campo va en el payload"
    En la variante `/{oid}` el `payload` lleva `oid`; en la variante `/byId/{id}` el `payload` lleva `id`. El mismo valor va también en la ruta.

## Response exitosa (HTTP 200)

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "PERSON_FETCH_DATA",
            "correlationId": "c5e2726b-225e-4958-90fe-7cb9291bdbfa",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-05-16T10:15:30.0000000Z"
                },
                {
                    "denaComponentId": "DENA_CORE",
                    "timestamp": "2026-05-16T10:15:32.0000000Z"
                }
            ]
        }
    },
    "payload": {
        "oid": "96C615E2-B24E-4449-B6B4-9551A4EE0A9D",
        "id": "99949992L",
        "name": "NOMBRE",
        "surname1": "APELLIDOUNO",
        "surname2": "ACTIVO"
    }
}
```

El `payload` de la respuesta es un objeto [`DN00Person`](../../../../semantica/data-retrieve/data/persona.md): `oid`, `id`, `name`, `surname1`, `surname2` y, si están disponibles, `contactInfo` y `lastChangeEvent`.

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "No person exists with oid=96C615E2-B24E-4449-B6B4-9551A4EE0A9D",
  "code" : -9999,
  "path" : "/person/api/admin/persons/96C615E2-B24E-4449-B6B4-9551A4EE0A9D"
}
```

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Persona encontrada y devuelta |
| `400` | Petición malformada o parámetros inválidos |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `404` | No existe ninguna persona con ese OID/ID |
| `500` | Error interno |
| `503` | Servicio no disponible |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
