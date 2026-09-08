# :material-account: PersonRef

## Deskribapena

DENAn erregistratutako **pertsona** baten erreferentzia. [DenaObjectRef](./object-ref.md)-en espezializazioa da (zehazki `DN00DENAObjectWithIDRefBase`-rena), beraz `oid` eta `id` eremuak heredatzen ditu eta ez du eremu propiorik gehitzen.

Klasea: `DN00PersonRef` (`@MarshallType(as="personRef")`).

!!! info "Gutxienez bat nahitaezkoa"
    `id` **edo** `oid` sartu behar da (edo biak). Biak sartzen badira, `oid`-ek du lehentasuna.

---

## JSON atributuak

| Eremua | Mota | Nahitaezkoa | Deskribapena |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Pertsonaren identifikatzaile bakarra DENAren pertsonen moduluan |
| `id` | `ID` | :material-close:* | Pertsonaren identifikatzaile administratiboa (NAN / NIF / NIE / Pasaportea) |

!!! note "* Gutxienez bat"
    `oid` edo `id`-etako bat gutxienez sartu behar da.

---

## Adibidea

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "id": "12345678A"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
