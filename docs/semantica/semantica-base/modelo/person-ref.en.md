# :material-account: PersonRef (DenaPersonRef)

## Description

Specialization of [DenaObjectRef](./object-ref.md) with minimal data about a **person** registered in DENA.

!!! info "At least one mandatory"
    Either `personId` **or** `objectOid` must be included (or both). If both are present, `objectOid` takes priority.

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `personId` | `ID` | :material-close:* | Administrative identifier of the person (DNI / NIF / NIE / Passport) |
| `objectOid` | `OID` | :material-close:* | Unique object identifier in DENA's person module |
| `createTS` | `TimeStamp` | :material-close: | Instant when the object was created in DENA |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Instant of last modification |
| `deleteTS` | `TimeStamp` | :material-close: | Instant when the object was deleted (if applicable) |
| `url` | `URL` | :material-close: | URL with the complete person data (requires authorization) |

---

## Full Example

```json
{
  "personId": "12345678A",
  "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "createTS": 1670374400,
  "lastUpdateTS": 1680500000,
  "deleteTS": null,
  "url": "https://interop.api.dena.eus/persons/6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Simplified usage

For most messages, the reduced format is sufficient:

```json
{
  "id": "12345678A",
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

where `id` maps to `personId` and `oid` maps to `objectOid`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
