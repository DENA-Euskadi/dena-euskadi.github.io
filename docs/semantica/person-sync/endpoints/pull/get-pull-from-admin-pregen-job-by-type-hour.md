# PERSON-SYNC — Get Pull from Admin Pregen Job (por tipo y hora)

Localiza un *job* pregenerado **sin conocer su OID**, indicando el tipo de fichero y la hora del día.

---

## ¿Cuándo se usa?

Es la forma habitual de trabajar con los pregenerados: como DENA los genera automáticamente cada hora, tu administración no conoce sus OIDs de antemano. Con este endpoint le pides a DENA "dame el pregenerado de tipo *X* de la hora *H*" y obtienes sus metadatos (incluido el estado), para después descargarlo con [Fetch Persons Pregen Export Asset](./fetch-persons-pregen-export-asset.md).

## Endpoint

```
POST /person-sync/api/admin/persons/sync/pregens
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

!!! note "Sin `{jobOid}` en la ruta"

    A diferencia de [Get Pull from Admin Pregen Job (por OID)](./get-pull-from-admin-pregen-job.md), aquí la ruta **no** lleva el `jobOid`: el job se identifica por los campos del `payload` (`jobType`, `exportType`, `fileFormat`, `hourOfDay`).

## Request

```json
{
    "context": {
        "message": {
            "type": "ADMIN_PERSON_PREGEN_JOB_FETCH_BY_TYPE_AND_HOUR",
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
        "jobType": "ALL_PERSONS",
        "exportType": "SYNC",
        "fileFormat": "CSV",
        "hourOfDay": "01"
    }
}
```

| Campo     | Tipo                                          | Obligatorio | Descripción |
|-----------|-----------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)   | ✅          | Contexto de la petición, incluyendo `message.type` con valor `ADMIN_PERSON_PREGEN_JOB_FETCH_BY_TYPE_AND_HOUR` |
| `payload` | [Payload](#payload)                           | ✅          | Payload de la petición |

## Payload

| Campo        | Tipo     | Obligatorio | Descripción |
|--------------|----------|-------------|-------------|
| `jobType`    | `String` | ✅          | Tipo de fichero pregenerado: <br> `ALL_PERSONS`: todas las personas de esa hora <br> `UPDATED_PERSONS_SINCE_LAST_SUCCESSFUL_JOB`: solo las personas actualizadas desde el último job procesado con éxito |
| `exportType` | `String` | ✅          | `data` (todos los datos de cada persona) o `sync` (solo timestamps de creación/actualización) |
| `fileFormat` | `String` | ✅          | Formato: `SQLITE`, `CSV`, `ZIP_OF_JSON` o `PARQUET` |
| `hourOfDay`  | `String` | ❌          | Hora del día en que se produjeron los cambios (`00`–`23`) |

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

El `oid` devuelto es el que puedes usar para descargar el asset o para consultar el job [por OID](./get-pull-from-admin-pregen-job.md). Los [estados del job](./get-pull-from-admin-pregen-job.md#estados-del-job) son los mismos.

## Response de error (HTTP 4xx/5xx)

```json
{
  "message": "No pregen job found for jobType=ALL_PERSONS hourOfDay=01",
  "code": -9999,
  "path": "/person-sync/api/admin/persons/sync/pregens"
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
| `404` | No hay pregenerado para los criterios indicados |
| `500` | Error interno |
| `503` | Servicio no disponible |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
