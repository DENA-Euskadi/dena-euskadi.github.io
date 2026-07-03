# :material-account: PersonRef

## Description

Object for referencing a person registered in DENA by their `oid` or `id` (NIF, NIE, etc.).

!!! info "At least one mandatory"

    Either `oid` **or** `id` must be included (or both).

---

## JSON attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Internal identifier of the person |
| `id` | `String` | :material-close:* | External identifier (NIF, NIE, etc.) |

---

## Example

```json
{
    "id": "12345678A",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
