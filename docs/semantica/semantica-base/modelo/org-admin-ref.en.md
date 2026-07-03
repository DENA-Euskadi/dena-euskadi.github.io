# :material-domain: OrgAdminRef

## Description

Object for referencing an administration by its `oid` or `id`.

!!! info "At least one mandatory"

    Either `oid` **or** `id` must be included (or both).

---

## JSON attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Internal identifier of the administration |
| `id` | `String` | :material-close:* | Textual identifier of the administration |

---

## Example

```json
{
    "id": "admin-A414",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
