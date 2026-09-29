# PERSON-SYNC — Fetch Persons Bespoke Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes/{jobOid}/asset
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (si OAuth está configurado)
```

Donde `{jobOid}` es el identificador del *job* que obtuviste al [crear la solicitud](./create-pull-from-admin-bespoke-job.md).

## Descripción

Descarga el resultado (el fichero de personas) de una solicitud de exportación **una vez que su estado es `FINISHED_OK`**. Es el **paso 3 y último** del flujo bespoke:

1. Crear la solicitud → obtienes un `jobOid`.
2. Consultar el estado periódicamente → esperas a `FINISHED_OK`.
3. **Descargar el fichero** (este endpoint).

!!! warning "Descarga solo cuando el job esté `FINISHED_OK`"

    Si intentas descargar el asset antes de que el job haya terminado, recibirás un error. Consulta primero el estado con [Get Pull from Admin Bespoke Job](./get-pull-from-admin-bespoke-job.md) hasta que devuelva `status: "FINISHED_OK"`.

## Request

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH",
            "correlationId": "aa645a6e-66a0-4c02-a00f-81d484a4296a",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp":"2026-06-11T14:55:01.7520000Z"
                }
            ]
        },
        "originAdmin": {
            "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
            "id": "admin-A414",
            "dir3Id": "EA0000001"
        },
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36"
    },
    "payload": {
        "jobOid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
    }
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Objeto de contexto de la petición, incluyendo `message.type` con valor `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` |
| `payload` | [Payload](#payload)                            | ✅          | Payload de la petición |


## Payload

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `jobOid` | `String` | ✅          | Identificador de la solicitud de la que descargar el resultado |

## Response exitosa (HTTP 200)

Datos binarios del fichero de exportación de personas en el formato solicitado (`application/octet-stream`). El formato concreto (CSV, SQLITE, ZIP_OF_JSON o PARQUET) es el que indicaste en el `exportSpec` al crear la solicitud.

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
