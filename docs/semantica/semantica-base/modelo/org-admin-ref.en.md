# :material-domain: OrgAdminRef (DenaOrgRef)

## Description

Specialization of [DenaObjectRef](./object-ref.md) with minimal data about an **administration**. Contains the information needed to uniquely identify an administration in DENA.

!!! info "Simplified sending"
    When an administration sends this information, **it only needs to send one of the identification fields** (`orgId`, `officialId` or `objectOid`). DENA can retrieve the rest from its internal entity directory.

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `orgId` | `ID` | :material-close:* | Administration identifier (e.g. NIF) |
| `officialId` | `ID` | :material-close:* | DIR3 code of the administration |
| `objectOid` | `OID` | :material-close:* | Unique object identifier in DENA's organization module |
| `createTS` | `TimeStamp` | :material-close: | Instant when the object was created in DENA |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Instant of last modification |
| `deleteTS` | `TimeStamp` | :material-close: | Instant when the object was deleted (if applicable) |
| `url` | `URL` | :material-close: | URL with the complete administration data |

!!! info "At least one mandatory"
    At least one of `orgId`, `officialId` or `objectOid` must be included.

---

## Example

```json
{
  "orgId": "S4833001C",
  "officialId": "EA0000001",
  "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "createTS": 1670374400,
  "lastUpdateTS": 1680500000,
  "url": "https://interop.api.dena.eus/orgs/6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Organizational structure

When sending an **organizational structure** (hierarchical organization chart), an `Array` of `DenaOrgRef` can be sent where:

- Element `[0]`: first level of the organization
- Element `[1]`: second level
- ...

```json
[
  { "orgId": "S4833001C", "officialId": "EA0000001" },
  { "orgId": "S4811001J", "officialId": "EA0041020" }
]
```

---

## Simplified usage

For most messages, the reduced format is sufficient:

```json
{
  "id": "admin-A414",
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

where `id` maps to `orgId` and `oid` maps to `objectOid`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
