# :material-account: PersonRef

## Description

Reference to a **person** registered in DENA. It is a specialization of [DenaObjectRef](./object-ref.md) (specifically of `DN00DENAObjectWithIDRefBase`), so it inherits the `oid` and `id` fields and adds no fields of its own.

Class: `DN00PersonRef` (`@MarshallType(as="personRef")`).

!!! info "At least one required"
    You must include `id` **or** `oid` (or both). If both are included, `oid` takes precedence.

---

## JSON attributes

| Field | Type | Required | Description |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Unique identifier of the person in DENA's persons module |
| `id` | `ID` | :material-close:* | Administrative identifier of the person (DNI / NIF / NIE / Passport) |

!!! note "* At least one"
    At least one of `oid` or `id` must be included.

---

## Example

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "id": "12345678A"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
