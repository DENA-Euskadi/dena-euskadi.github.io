# PERSON-SYNC — Fetch Persons Bespoke Export Asset

## Endpoint

```
POST /person-sync/api/admin/persons/sync/bespokes/{jobOid}/asset
Content-Type: application/json
Accept: application/octet-stream
Authorization: Bearer <token> (if OAuth is configured)
```

Where `{jobOid}` is the identifier of the *job* you obtained when [creating the request](./create-pull-from-admin-bespoke-job.md).

## Description

Downloads the result (the persons file) of an export request **once its status is `FINISHED_OK`**. It is the **third and final step** of the bespoke flow:

1. Create the request → you get a `jobOid`.
2. Poll the status → wait for `FINISHED_OK`.
3. **Download the file** (this endpoint).

!!! warning "Download only when the job is `FINISHED_OK`"

    If you try to download the asset before the job has finished, you will get an error. First check the status with [Get Pull from Admin Bespoke Job](./get-pull-from-admin-bespoke-job.md) until it returns `status: "FINISHED_OK"`.

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

| Field     | Type                                           | Mandatory | Description |
|-----------|------------------------------------------------|:---------:|-------------|
| `context` | [Context](../../../semantica-base/index.md)          | ✅        | Request context object, including `message.type` with value `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` |
| `payload` | [Payload](#payload)                            | ✅        | Request payload |


## Payload

| Field    | Type     | Mandatory | Description |
|----------|----------|:---------:|-------------|
| `jobOid` | `String` | ✅        | Identifier of the request whose result is to be downloaded |

## Successful response (HTTP 200)

Binary data of the user export file in the requested format.

## Error response (HTTP 4xx/5xx)

```json
{
  "message" : "[ group=1 code=2 code=2 severity=FATAL ]: Persistence error when executing 'current' method: UNKNOWN ERROR!",
  "code" : -9999,
  "path" : "/person-sync/api/admin/persons/sync/bespokes/F74724F6-65F6-4E01-B215-AB8CDA3FC42B/asset"
}
```

---

## HTTP codes

| Code | Meaning |
|------|---------|
| `200` | Data returned successfully (may be an empty list) |
| `400` | Malformed request or invalid parameters |
| `401` | Unauthorised (invalid or expired token) |
| `403` | Forbidden (insufficient permissions) |
| `404` | Person not found |
| `500` | Internal error |
| `503` | Service unavailable |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
