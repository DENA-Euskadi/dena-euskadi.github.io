# :material-cube-outline: DenaObjectRef

## Description

Base type for referencing any **object** in DENA (person, consent, administration, etc.). Every object in DENA has at least a unique identifier, lifecycle timestamps and an access URL.

`DenaObjectRef` is the base class from which [DenaOrgRef](./org-admin-ref.md) and [DenaPersonRef](./person-ref.md) inherit.

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `objectOid` | `OID` | :material-check: | Unique object identifier in DENA |
| `createTS` | `TimeStamp` | :material-close: | Instant when the object was created in DENA |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Instant of the last modification |
| `deleteTS` | `TimeStamp` | :material-close: | Instant when the object was deleted (if applicable) |
| `url` | `URL` | :material-close: | URL with the complete object data (requires authorization) |

---

## Example

```json
{
  "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "createTS": 1670374400,
  "lastUpdateTS": 1680500000,
  "deleteTS": null,
  "url": "https://interop.api.dena.eus/objects/6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Specializations

<div class="grid cards" markdown>

-   :material-domain:{ .lg .middle } **DenaOrgRef**

    ---

    Extends DenaObjectRef with administration data.

    [:octicons-arrow-right-24: View model](./org-admin-ref.md)

-   :material-account:{ .lg .middle } **DenaPersonRef**

    ---

    Extends DenaObjectRef with person data.

    [:octicons-arrow-right-24: View model](./person-ref.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
