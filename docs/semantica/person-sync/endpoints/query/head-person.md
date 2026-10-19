# PERSON-SYNC — Consulta de Persona (Head)

## Endpoint

Comprueba si una persona **existe** sin devolver sus datos. Dos variantes, por **OID** o por **ID**:

```
POST /person/api/admin/persons/{oid}:head
POST /person/api/admin/persons/byId/{id}:head
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

!!! note "Verbo custom en la ruta (AIP-136)"
    El sufijo `:head` sigue la convención [AIP-136](https://google.aip.dev/136) (Google API Improvement Proposals) para métodos personalizados en la ruta. Aunque se llame *head*, la operación es un `POST` (lleva cuerpo con el contexto de la petición).

## Descripción

Verifica la **existencia** de una persona en DENA sin recuperar sus datos completos. Útil para comprobaciones rápidas antes de enviar SRMD. Forma parte de la **Consulta de Personas** del servicio `person-admin-rest`.

## Request

```json
{
    "context": {
        "message": {
            "type": "PERSON_ADMIN_HEAD",
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
| `context` | [Context](../../../semantica-base/index.md)    | ✅          | Objeto de contexto de la petición, con `message.type` con valor `PERSON_ADMIN_HEAD` |
| `payload` | [Payload](#payload)                            | ✅          | Referencia a la persona a comprobar |

## Payload

| Campo | Tipo     | Obligatorio | Descripción |
|-------|----------|-------------|-------------|
| `oid` | `String` | ✅ (variante por OID) | OID de la persona en DENA |
| `id`  | `String` | ✅ (variante por ID)  | NIF/NIE de la persona |

## Response exitosa (HTTP 200)

El `payload` de la respuesta es un **booleano** que indica si la persona existe:

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "PERSON_ADMIN_HEAD",
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
    "payload": true
}
```

| `payload` | Significado |
|-----------|-------------|
| `true`    | La persona existe en DENA |
| `false`   | La persona no existe en DENA |

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "The request could not be processed",
  "code" : -9999,
  "path" : "/person/api/admin/persons/96C615E2-B24E-4449-B6B4-9551A4EE0A9D:head"
}
```

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Comprobación realizada; el booleano del `payload` indica si existe |
| `400` | Petición malformada o parámetros inválidos |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `500` | Error interno |
| `503` | Servicio no disponible |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
