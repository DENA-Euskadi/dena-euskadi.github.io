# PERSON-SYNC — Get Pull from Admin Bespoke Job

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes/12345-ABC-DEFG
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

## Descripción

Consulta el estado de una solicitud de exportación.

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
        "messageType": "GET_PULL_ADMIN_BESPOKE",
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
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Objeto de contexto de la petición, incluyendo messageType con valor `GET_PULL_ADMIN_BESPOKE` |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |


## Data

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `jobOid` | `String` | ✅          | Identificador de la solicitud sobre la que consultar el estado |

## Response exitosa (HTTP 200)

```json
{
    "data": {
        "job": {
            "admin": {
                "id": "admin-A414",
                "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
            },
            "entityVersion": 1,
            "exportResultAssets": [
                {
                    "exportContentSpec": {
                        "personExportSpec": "sync",
                        "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
                        "syncEvent": "CREATED",
                        "exportFileFormat": "CSV"
                    },
                    "fileStoreItemOid": "652DB18C-DBD6-4B6C-A520-7D808A3CB8FE"
                }
            ],
            "exportSpec": {
                "personExportSpec": "sync",
                "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
                "exportFileFormat": "CSV"
            },
            "finishedAt": "2026-05-26T23:28:00.1193311Z",
            "numericId": 0,
            "oid": "0E6A859E-2472-4A86-9111-2AB7DB70DB9B",
            "processingAttempts": 1,
            "registeredAt": "2026-05-26T23:26:06.2436446Z",
            "startedAt": "2026-05-26T23:28:00.0384188Z",
            "status": "FINISHED_OK",
            "trackingInfo": {
                "createDate": "2026-05-26T23:26:06.2436450Z",
                "creatorUserCode": "mock-user-login",
                "creatorUserOid": "983cf052-e4fc-4a45-86de-3d53f4518813",
                "lastUpdate": "2026-05-26T23:28:00.1234540Z",
                "lastUpdaterUserCode": "mock-user-login",
                "lastUpdaterUserOid": "6ed87233-57e3-4d1e-80e7-90eba68db06a"
            }
        }
    }
}
```

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "The job with oid=F74724F6-65F6-4E01-B215-AB8CDA3FC42B does NOT exist",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes/F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
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
