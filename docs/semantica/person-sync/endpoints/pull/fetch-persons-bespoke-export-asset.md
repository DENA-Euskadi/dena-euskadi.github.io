# PERSON-SYNC — Fetch Persons Bespoke Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (si OAuth está configurado)
```

## Descripción

Descarga el resultado de una petición de exportación de personas usuarias de DENA en el formato indicado.

## Request

```json
{
    "context": {
        "interopRouteData": [
            {
                "denaComponentId": "DENA_POSTMAN",
                "timestamp":"2026-06-11T14:55:01.7520000Z"
            }
        ],
        "messageCorrelationId": "aa645a6e-66a0-4c02-a00f-81d484a4296a",
        "messageType": "FETCH_BESPOKE_EXPORT_ASSET",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originPartyId": "ADMIN-001",
        "destinationPartyId": "DENA_INTEROP"
    },
    "data": {
        "jobOid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
    }
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md)          | ✅          | Objeto de contexto de la petición, incluyendo messageType con valor `FETCH_BESPOKE_EXPORT_ASSET` |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |


## Data

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `jobOid` | `String` | ✅          | Identificador de la solicitud de la que descargar el resultado |

## Response exitosa (HTTP 200)

Datos binaros del fichero de exportación de usuario en el formato solicitado

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes/F74724F6-65F6-4E01-B215-AB8CDA3FC42B/asset"
}
```

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

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
