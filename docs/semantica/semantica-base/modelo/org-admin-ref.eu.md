# :material-domain: OrgAdminRef (DenaOrgRef)

## Deskribapena

[DenaObjectRef](./object-ref.md)-ren espezializazioa, **administrazio** bati buruzko gutxieneko datuekin. DENAn administrazio bat modu bakarra identifikatzeko beharrezko informazioa dauka.

!!! info "Bidalketa sinplifikatua"
    Administrazio batek informazio hau bidaltzen duenean, **nahikoa da identifikazio-eremu bat bidaltzea** (`orgId`, `officialId` edo `objectOid`). DENAk gainerakoa bere barne-direktorio entitateetatik lor dezake.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `orgId` | `ID` | :material-close:* | Administrazioaren identifikatzailea (adib. NIF) |
| `officialId` | `ID` | :material-close:* | Administrazioaren DIR3 kodea |
| `objectOid` | `OID` | :material-close:* | DENAren antolaketa-moduluko objektuaren identifikatzaile bakarra |
| `createTS` | `TimeStamp` | :material-close: | Objektua DENAn sortu zen unea |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Azken aldaketaren unea |
| `deleteTS` | `TimeStamp` | :material-close: | Objektua ezabatu zen unea (aplikagarria bada) |
| `url` | `URL` | :material-close: | Administrazioaren datu osoen URLa |

!!! info "Gutxienez bat derrigorrezkoa"
    `orgId`, `officialId` edo `objectOid`-etik gutxienez bat sartu behar da.

---

## Adibidea

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

## Erabilera sinplifikatua

Mezu gehienetarako, formatu murriztua nahikoa da:

```json
{
  "id": "admin-A414",
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

non `id` → `orgId` eta `oid` → `objectOid`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
