# PERSON-SYNC — Get Pull from Admin Pregen Job (por OID)

Consulta el estado y los metadatos de un *job* pregenerado a partir de su identificador (`jobOid`).

---

## ¿Cuándo se usa?

Los ficheros **pregenerados** los produce DENA automáticamente cada hora (no los solicita la administración). Este endpoint permite **consultar un job pregenerado concreto** cuando ya conoces su `jobOid`, para saber su estado y de qué exportación dispone antes de descargarlo con [Fetch Persons Pregen Export Asset](./fetch-persons-pregen-export-asset.md).

!!! tip "¿No conoces el OID?"

    Si no tienes el `jobOid` y quieres localizar el pregenerado de una hora/tipo concretos, usa [Get Pull from Admin Pregen Job (por tipo y hora)](./get-pull-from-admin-pregen-job-by-type-hour.md).

## Endpoint

```
POST /person-sync/api/admin/persons/sync/pregens/{jobOid}
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

Donde `{jobOid}` es el identificador del job pregenerado que quieres consultar.

## Request

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_PREGEN_JOB_FETCH",
            "correlationId": "0777f936-4c31-43b5-81ee-fdf4d708f147",
            "interopRouteData": [
                {
                    "denaComponentId": "ADMIN",
                    "timestamp": "2026-06-10T15:37:57.5530000Z"
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

| Campo     | Tipo                                          | Obligatorio | Descripción |
|-----------|-----------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)   | ✅          | Contexto de la petición, incluyendo `message.type` con valor `ADMIN_PERSON_PREGEN_JOB_FETCH` |
| `payload` | [Payload](#payload)                           | ✅          | Payload de la petición |

## Payload

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `jobOid` | `String` | ✅          | Identificador del job pregenerado a consultar |

## Response exitosa (HTTP 200)

```json
{
    "payload": {
        "job": {
            "oid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B",
            "registeredAt": "2026-05-26T01:00:00.0284171Z",
            "status": "FINISHED_OK"
        }
    }
}
```

| Campo del job | Descripción |
|---------------|-------------|
| `oid` | Identificador del job pregenerado |
| `registeredAt` | Momento en que DENA registró/generó el job |
| `status` | Estado del job (ver [Estados](#estados-del-job)) |

## Estados del job

| Estado | Significado |
|--------|-------------|
| `REGISTERED` | Registrado, pendiente de procesar |
| `BEING_PROCESSED` | En proceso de generación |
| `FINISHED_OK` | Terminado correctamente: el fichero está listo para descargar |
| `FINISHED_NOK` | Terminado con error |
| `FINISHED_NOK_TOO_MANY_ATTEMPTS` | Terminado con error tras agotar los reintentos |

## Response de error (HTTP 4xx/5xx)

```json
{
  "message": "The job with oid=F74724F6-65F6-4E01-B215-AB8CDA3FC42B does NOT exist",
  "code": -9999,
  "path": "/person-sync/api/admin/persons/sync/pregens/F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
}
```

---

## Códigos HTTP

| Código | Significado |
|--------|-------------|
| `200` | Job devuelto correctamente |
| `400` | Petición malformada o parámetros inválidos |
| `401` | No autorizado (token inválido o expirado) |
| `403` | Prohibido (sin permisos) |
| `404` | Job no encontrado |
| `500` | Error interno |
| `503` | Servicio no disponible |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
