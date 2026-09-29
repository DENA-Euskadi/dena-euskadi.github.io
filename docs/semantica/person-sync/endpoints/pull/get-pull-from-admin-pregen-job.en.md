# PERSON-SYNC — Get Pull from Admin Pregen Job (by OID)

Queries the status and metadata of a pre-generated *job* from its identifier (`jobOid`).

---

## When is it used?

The **pre-generated** files are produced automatically by DENA every hour (the administration does not request them). This endpoint lets you **query a specific pre-generated job** when you already know its `jobOid`, to check its status and which export it holds before downloading it with [Fetch Persons Pregen Export Asset](./fetch-persons-pregen-export-asset.md).

!!! tip "Don't know the OID?"

    If you don't have the `jobOid` and want to locate the pre-generated file for a specific hour/type, use [Get Pull from Admin Pregen Job (by type and hour)](./get-pull-from-admin-pregen-job-by-type-hour.md).

## Endpoint

```
POST /person-sync/api/admin/persons/sync/pregens/{jobOid}
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (if OAuth is configured)
```

Where `{jobOid}` is the identifier of the pre-generated job you want to query.

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

| Field     | Type                                          | Required | Description |
|-----------|-----------------------------------------------|----------|-------------|
| `context` | [Context](../../../semantica-base/index.md)   | ✅       | Request context, including `message.type` with value `ADMIN_PERSON_PREGEN_JOB_FETCH` |
| `payload` | [Payload](#payload)                           | ✅       | Request payload |

## Payload

| Field    | Type     | Required | Description |
|----------|----------|----------|-------------|
| `jobOid` | `String` | ✅       | Identifier of the pre-generated job to query |

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

| Job field | Description |
|-----------|-------------|
| `oid` | Identifier of the pre-generated job |
| `registeredAt` | When DENA registered/generated the job |
| `status` | Job status (see [Statuses](#job-statuses)) |

## Job statuses

| Status | Meaning |
|--------|---------|
| `REGISTERED` | Registered, pending processing |
| `BEING_PROCESSED` | Being generated |
| `FINISHED_OK` | Finished correctly: the file is ready to download |
| `FINISHED_NOK` | Finished with error |
| `FINISHED_NOK_TOO_MANY_ATTEMPTS` | Finished with error after exhausting retries |

## Error response (HTTP 4xx/5xx)

```json
{
  "message": "The job with oid=F74724F6-65F6-4E01-B215-AB8CDA3FC42B does NOT exist",
  "code": -9999,
  "path": "/person-sync/api/admin/persons/sync/pregens/F74724F6-65F6-4E01-B215-AB8CDA3FC42B"
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
| `404` | Job not found |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
