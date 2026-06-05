# PERSON-SYNC — Fetch Persons Pregen Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/pregens/asset
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (si OAuth está configurado)
```

## Descripción

Descarga un listado pregenerado de personas usuarias de DENA. Estos se generan cada hora del dia con los cambios de personas que se han producido en dicha hora.

## Request

```json
{
    "context": {
        <Objeto de Contexto>
    },
    "data": {
        "type": "fetchPreGenExportAssetRequestPayload",
        "jobType": "ALL_PERSONS",
        "exportType": "SYNC",
        "fileFormat": "CSV",
        "hourOfDay": "01"
    }
}
```

| Campo     | Tipo                                           | Obligatorio | Descripción |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../semantica-base/index.md) | ✅          | Objeto de contexto de la petición |
| `data`    | [Data](#data)                                  | ✅          | Payload de la petición |


## Data

| Campo        | Tipo     | Obligatorio | Descripción |
|--------------|----------|-------------|-------------|
| `type`       | `String` | ✅          | `"fetchPreGenExportAssetRequestPayload"` |
| `jobType`    | `String` | ✅          | Tipo de fichero pregenerado. Valores posibles: <br> `ALL_PERSONS`: Todas las personas de cada hora <br> `UPDATED_PERSONS_SINCE_LAST_SUCCESSFUL_JOB`: Solo las personas actualizadas cada hora |
| `exportType` | `String` | ✅          | `data` (Incluye todos los datos de cada persona) <br> `sync` (Solo incluye timestamps de creación/actualización) |
| `fileFormat` | `String` | ✅          | Formato en el que descargar los datos. Valores posibles: `SQLITE`, `CSV`, `ZIP_OF_JSON` o `PARQUET` |
| `hourOfDay`  | `String` | ✅          | Hora del día en que se han producido los cambios en las personas (00-23) |

## Response exitosa (HTTP 200)

Datos binaros del fichero de exportación de usuario en el formato solicitado

## Response de error (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/pregens/asset"
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
