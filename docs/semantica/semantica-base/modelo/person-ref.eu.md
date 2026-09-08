# :material-account: PersonRef (DenaPersonRef)

## Deskribapena

[DenaObjectRef](./object-ref.md)-ren espezializazioa, DENAn erregistratutako **pertsona** bati buruzko gutxieneko datuekin.

!!! info "Gutxienez bat derrigorrezkoa"
    `personId` **edo** `objectOid` sartu behar da (edo biak). Biak sartzen badira, `objectOid`-ek lehentasuna du.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `personId` | `ID` | :material-close:* | Pertsonaren identifikatzaile administratiboa (NAN / IFK / AIZ / Pasaportea) |
| `objectOid` | `OID` | :material-close:* | DENAren pertsona-moduluko objektuaren identifikatzaile bakarra |
| `createTS` | `TimeStamp` | :material-close: | Objektua DENAn sortu zen unea |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Azken aldaketaren unea |
| `deleteTS` | `TimeStamp` | :material-close: | Objektua ezabatu zen unea (aplikagarria bada) |
| `url` | `URL` | :material-close: | Pertsonaren datu osoen URLa (baimena behar du) |

---

## Adibide osoa

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

## Erabilera sinplifikatua

Mezu gehienetarako, formatu murriztua nahikoa da:

```json
{
  "id": "12345678A",
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

non `id` → `personId` eta `oid` → `objectOid`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
