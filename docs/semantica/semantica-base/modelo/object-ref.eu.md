# :material-cube-outline: DenaObjectRef

## Deskribapena

DENAko edozein **objektu** erreferentziatzeko oinarrizko mota (pertsona, baimena, administrazioa, etab.). DENAko objektu orok gutxienez identifikatzaile bakarra, bizi-zikloko timestamp-ak eta sarbide-URL bat ditu.

`DenaObjectRef` [DenaOrgRef](./org-admin-ref.md) eta [DenaPersonRef](./person-ref.md) heredatzen duten oinarrizko klasea da.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `objectOid` | `OID` | :material-check: | Objektuaren identifikatzaile bakarra DENAn |
| `createTS` | `TimeStamp` | :material-close: | Objektua DENAn sortu zen unea |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Azken aldaketaren unea |
| `deleteTS` | `TimeStamp` | :material-close: | Objektua ezabatu zen unea (aplikagarria bada) |
| `url` | `URL` | :material-close: | Objektuaren datu osoen URLa (baimena behar du) |

---

## Adibidea

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

## Espezializazioak

<div class="grid cards" markdown>

-   :material-domain:{ .lg .middle } **DenaOrgRef**

    ---

    DenaObjectRef hedatzen du administrazioaren datuekin.

    [:octicons-arrow-right-24: Eredua ikusi](./org-admin-ref.md)

-   :material-account:{ .lg .middle } **DenaPersonRef**

    ---

    DenaObjectRef hedatzen du pertsonaren datuekin.

    [:octicons-arrow-right-24: Eredua ikusi](./person-ref.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
