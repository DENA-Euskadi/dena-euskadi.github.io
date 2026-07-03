# :material-tag: DataTypeRef

## Description

Object for referencing a data type managed by DENA (e.g. Record, Notification, Payment...).

!!! info "At least one mandatory"

    Either `oid` **or** `id` must be included (or both). If both are included, `oid` takes priority.

---

## JSON attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Internal identifier of the data type |
| `id` | `String` | :material-close:* | Textual identifier of the data type |

---

## Example

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Standard values of `id`

| `id` | Data type |
|---|---|
| `RECORDS` | Records |
| `NOTICES` | Notifications |
| `REGISTRY` | Official registrations |
| `PAYMENTS` | Payments |
| `SCHEDULE` | Appointments |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
