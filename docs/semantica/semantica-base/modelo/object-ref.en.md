# :material-cube-outline: DenaObjectRef

## Description

Base type to reference any **object** in DENA (person, administration, data type, etc.). Every object reference in DENA is identified, at a minimum, by its OID.

The reference hierarchy has two base levels:

| Level | Class | Provides |
|---|---|---|
| Reference by OID | `DN00DENAObjectRefBase` | The `oid` field |
| Reference by OID + ID | `DN00DENAObjectWithIDRefBase` | Adds the `id` field |

The concrete references inherit from `DN00DENAObjectWithIDRefBase`: [PersonRef](./person-ref.md), [OrgAdminRef](./org-admin-ref.md) and [DataTypeRef](./data-type-ref.md).

---

## JSON attributes

| Field | Type | Required | Description |
|---|---|:---:|---|
| `oid` | `OID` | :material-check: | Unique identifier of the object in DENA |

!!! info "`id` comes with the specialization"
    `DN00DENAObjectRefBase` only defines `oid`. The `id` field (business identifier) is provided by the intermediate class `DN00DENAObjectWithIDRefBase`, from which the concrete references inherit.

---

## Example

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Specializations

<div class="grid cards" markdown>

-   :material-account:{ .lg .middle } **PersonRef**

    ---

    Reference to a person (`oid` + `id`).

    [:octicons-arrow-right-24: View model](./person-ref.md)

-   :material-domain:{ .lg .middle } **OrgAdminRef**

    ---

    Reference to an administration (`oid` + `id` + `dir3Id`).

    [:octicons-arrow-right-24: View model](./org-admin-ref.md)

-   :material-tag-outline:{ .lg .middle } **DataTypeRef**

    ---

    Reference to a data type (`oid` + `id`).

    [:octicons-arrow-right-24: View model](./data-type-ref.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
