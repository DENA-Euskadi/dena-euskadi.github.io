# PERSON-SYNC — Fetch Persons Pregen Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/pregens/asset
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (if OAuth is configured)
```

## Description

Downloads a pre-generated list of DENA users. These are generated every hour of the day with the person changes that occurred during that hour.

## Request

```json
{
    "context": {
        "interopRouteData": [
            {
                "denaComponentId": "DENA_POSTMAN",
                "timestamp":"2026-06-10T15:37:57.5530000Z"
            }
        ],
        "messageCorrelationId": "0777f936-4c31-43b5-81ee-fdf4d708f147",
        "messageType": "FETCH_PREGEN_EXPORT_ASSET",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
        "originPartyId": "ADMIN-001",
        "destinationPartyId": "DENA_INTEROP"
    },
    "data": {
        "jobType": "ALL_PERSONS",
        "exportType": "SYNC",
        "fileFormat": "CSV",
        "hourOfDay": "01"
    }
}
```

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|-------------|-------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅          | Request context object, including messageType with value `FETCH_PREGEN_EXPORT_ASSET` |
| `data`    | [Data](#data)                                  | ✅          | Request payload |


## Data

| Field        | Type     | Mandatory | Description |
|--------------|----------|-------------|-------------|
| `type`       | `String` | ✅          | `"fetchPreGenExportAssetRequestPayload"` |
| `jobType`    | `String` | ✅          | Type of pre-generated file. Possible values: <br> `ALL_PERSONS`: All persons for each hour <br> `UPDATED_PERSONS_SINCE_LAST_SUCCESSFUL_JOB`: Only persons updated each hour |
| `exportType` | `String` | ✅          | `data` (Includes all data for each person) <br> `sync` (Only includes creation/update timestamps) |
| `fileFormat` | `String` | ✅          | Format in which to download the data. Possible values: `SQLITE`, `CSV`, `ZIP_OF_JSON` or `PARQUET` |
| `hourOfDay`  | `String` | ❌          | Hour of the day when person changes occurred (00-23) |

## Successful response (HTTP 200)

Binary data of the user export file in the requested format

## Error response (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/pregens/asset"
}
```

---

## HTTP codes

| Code | Meaning |
|--------|-------------|
| `200` | Data returned successfully (can be an empty list) |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorized (invalid or expired token) |
| `403` | Forbidden (no permissions) |
| `404` | Person not found |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
