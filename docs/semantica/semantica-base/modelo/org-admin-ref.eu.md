# :material-domain: OrgAdminRef

## Deskribapena

**Administrazio** baten erreferentzia. [DenaObjectRef](./object-ref.md)-en espezializazioa da (zehazki `DN00DENAObjectWithIDRefBase`-rena): `oid` eta `id` heredatzen ditu, eta `dir3Id` eremu propioa gehitzen du.

Klasea: `DN00OrgAdminRef`.

!!! info "Bidalketa sinplifikatua"
    Administrazio batek erreferentzia hau bidaltzen duenean, identifikazio-eremuetako bat nahikoa da (`oid`, `id` edo `dir3Id`). DENAk gainerakoa bere barneko entitate-direktoriotik lortzen du.

---

## JSON atributuak

| Eremua | Mota | Nahitaezkoa | Deskribapena |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Administrazioaren identifikatzaile bakarra DENAren antolakuntza-moduluan |
| `id` | `ID` | :material-close:* | Administrazioaren identifikatzailea (adib. NIF) |
| `dir3Id` | `ID` | :material-close:* | Administrazioaren DIR3 kodea |

!!! note "* Gutxienez bat"
    `oid`, `id` edo `dir3Id`-etako bat gutxienez sartu behar da.

---

## Adibidea

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "id": "S4833001C",
  "dir3Id": "EA0000001"
}
```

---

## Antolakuntza-egitura

**Antolakuntza-egitura** bat (organigrama hierarkikoa) bidali behar denean, erreferentzia-`Array` bat bidal daiteke, non ordenak maila hierarkikoa markatzen duen:

- `[0]` elementua: antolakuntzaren lehen maila
- `[1]` elementua: bigarren maila
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
