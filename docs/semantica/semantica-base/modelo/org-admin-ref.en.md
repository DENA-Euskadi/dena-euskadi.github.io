# :material-domain: OrgAdminRef

## Description

Reference to an **administration**. It is a specialization of [DenaObjectRef](./object-ref.md) (specifically of `DN00DENAObjectWithIDRefBase`): it inherits `oid` and `id`, and adds its own field `dir3Id`.

Class: `DN00OrgAdminRef`.

!!! info "Simplified submission"
    When an administration sends this reference, one of the identification fields is enough (`oid`, `id` or `dir3Id`). DENA obtains the rest from its internal entity directory.

---

## JSON attributes

| Field | Type | Required | Description |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Unique identifier of the administration in DENA's organization module |
| `id` | `ID` | :material-close:* | Identifier of the administration (e.g. NIF) |
| `dir3Id` | `ID` | :material-close:* | DIR3 code of the administration |

!!! note "* At least one"
    At least one of `oid`, `id` or `dir3Id` must be included.

---

## Example

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "id": "S4833001C",
  "dir3Id": "EA0000001"
}
```

---

## Organizational structure

When an **organizational structure** (hierarchical org chart) needs to be sent, an `Array` of references can be sent where the order marks the hierarchy level:

- Element `[0]`: first level of the organization
- Element `[1]`: second level
- ...

```json
[
  { "id": "S4833001C", "dir3Id": "EA0000001" },
  { "id": "S4811001J", "dir3Id": "EA0041020" }
]
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
