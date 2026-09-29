# PERSON-SYNC — Get Pull from Admin Pregen Job (by type and hour)

Locates a pre-generated *job* **without knowing its OID**, by indicating the file type and the hour of the day.

---

## When is it used?

This is the usual way of working with pre-generated files: since DENA generates them automatically every hour, your administration does not know their OIDs in advance. With this endpoint you ask DENA "give me the pre-generated file of type *X* for hour *H*" and you get its metadata (including the status), to then download it with [Fetch Persons Pregen Export Asset](./fetch-persons-pregen-export-asset.md).

## Endpoint

```
POST /person-sync/api/admin/persons/sync/pregens
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

!!! note "No `{jobOid}` in the route"

    Unlike [Get Pull from Admin Pregen Job (by OID)](./get-pull-from-admin-pregen-job.md), here the route does **not** carry the `jobOid`: the job is identified by the `payload` fields (`jobType`, `exportType`, `fileFormat`, `hourOfDay`).

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

| Field     | Type                                          | Required | Description |
|-----------|-----------------------------------------------|----------|-------------|
| `context` | [Context](../../../semantica-base/index.md)   | ✅       | Request context, including `message.type` with value `ADMIN_PERSON_PREGEN_JOB_FETCH_BY_TYPE_AND_HOUR` |
| `payload` | [Payload](#payload)                           | ✅       | Request payload |

## Payload

| Field        | Type     | Required | Description |
|--------------|----------|----------|-------------|
| `jobType`    | `String` | ✅       | Pre-generated file type: <br> `ALL_PERSONS`: all people from that hour <br> `UPDATED_PERSONS_SINCE_LAST_SUCCESSFUL_JOB`: only people updated since the last successfully processed job |
| `exportType` | `String` | ✅       | `data` (all data of each person) or `sync` (only creation/update timestamps) |
| `fileFormat` | `String` | ✅       | Format: `SQLITE`, `CSV`, `ZIP_OF_JSON` or `PARQUET` |
| `hourOfDay`  | `String` | ❌       | Hour of the day when the changes occurred (`00`–`23`) |

## Successful response (HTTP 200)

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

The returned `oid` is the one you can use to download the asset or to query the job [by OID](./get-pull-from-admin-pregen-job.md). The [job statuses](./get-pull-from-admin-pregen-job.md#job-statuses) are the same.

## Error response (HTTP 4xx/5xx)

```json
{
  "message": "No pregen job found for jobType=ALL_PERSONS hourOfDay=01",
  "code": -9999,
  "path": "/person-sync/api/admin/persons/sync/pregens"
}
```

---

## HTTP codes

| Code | Meaning |
|------|---------|
| `200` | Job returned correctly |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorized (invalid or expired token) |
| `403` | Forbidden (no permissions) |
| `404` | No pre-generated file for the given criteria |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
