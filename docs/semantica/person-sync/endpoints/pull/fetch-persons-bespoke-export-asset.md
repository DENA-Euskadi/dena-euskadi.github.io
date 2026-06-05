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
        <Objeto de Contexto>
    },
    "data": {
        "operation": "fetchBespokeExportAssetRequestPayload",
        "jobOid": "F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
    }
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md) | ✅          | Objeto de contexto de la petición |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |


## Data

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `type`   | `String` | ✅          | `"fetchBespokeExportAssetRequestPayload"` |
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
