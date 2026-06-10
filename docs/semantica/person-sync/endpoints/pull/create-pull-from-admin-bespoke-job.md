# PERSON-SYNC — Create Pull from Admin Bespoke Job

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (si OAuth está configurado)
```

## Descripción

Crea una solicitud de exportación de un listado de personas usuarias de DENA para ser procesado asincronamente.

## Request

```json
{
    "context": {
        <Objeto de Contexto>
    },
    "data": {
        "type": "createPullAdminBespokeRequestPayload",
        "orgAdminRef": {
            "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
        },
        "exportSpec": {
            "personExportSpec": "sync",
            "exportFileFormat": "CSV",
            "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
            "syncEvent": "CREATED"
        }
    }
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md) | ✅          | Objeto de contexto de la petición |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |


## Data

| Campo         | Tipo     | Obligatorio | Descripción |
|---------------|----------|-------------|-------------|
| `type`        | `String` | ✅ | `"createPullAdminBespokeRequestPayload"` |
| `orgAdminRef` | [OrgAdminRef](../../../semantica-base/modelo/org-admin-ref.md) | ✅ | Referencia a la administración |
| `exportSpec`  | [ExportSpec](../../modelo/pull/export-spec.md) | ✅ | Especificación de las personas que exportar y el formato de destino |

## Response exitosa (HTTP 200)

```json
{
    "data": {
        "job": {
            "admin": {
                "id": "admin-A414",
                "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
            },
            "entityVersion": 0,
            "exportSpec": {
                "personExportSpec": "sync",
                "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
                "syncEvent": "CREATED",
                "exportFileFormat": "CSV"
            },
            "numericId": 0,
            "oid": "7F4C8F2A-03AB-4864-AC19-7D2E2FAD7FC2",
            "processingAttempts": 0,
            "registeredAt": "2026-05-26T23:23:28.0284171Z",
            "status": "REGISTERED",
            "trackingInfo": {
                "createDate": "2026-05-26T23:23:28.4973937Z",
                "creatorUserCode": "mock-user-login",
                "creatorUserOid": "09587694-e8ad-4d45-8f0c-0000f753d716",
                "lastUpdate": "2026-05-26T23:23:28.4973937Z"
            }
        }
    }
}
```

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "400 BAD_REQUEST \"request doesn't contain neither [admin]  oid or id \"",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes"
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
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
