# :material-cube-outline: DenaObjectRef

## Deskribapena

DENAren edozein **objektu** erreferentziatzeko oinarrizko mota (pertsona, administrazioa, datu mota, etab.). DENAn objektu-erreferentzia oro, gutxienez, bere OIDaren bidez identifikatzen da.

Erreferentzien hierarkiak bi oinarri-maila ditu:

| Maila | Klasea | Zer ematen du |
|---|---|---|
| OID bidezko erreferentzia | `DN00DENAObjectRefBase` | `oid` eremua |
| OID + ID bidezko erreferentzia | `DN00DENAObjectWithIDRefBase` | `id` eremua gehitzen du |

Erreferentzia konkretuak `DN00DENAObjectWithIDRefBase`-tik heredatzen dira: [PersonRef](./person-ref.md), [OrgAdminRef](./org-admin-ref.md) eta [DataTypeRef](./data-type-ref.md).

---

## JSON atributuak

| Eremua | Mota | Nahitaezkoa | Deskribapena |
|---|---|:---:|---|
| `oid` | `OID` | :material-check: | Objektuaren identifikatzaile bakarra DENAn |

!!! info "`id` espezializazioarekin dator"
    `DN00DENAObjectRefBase`-k `oid` bakarrik definitzen du. `id` eremua (negozio-identifikatzailea) `DN00DENAObjectWithIDRefBase` tarteko klaseak ematen du, eta hortik heredatzen dute erreferentzia konkretuek.

---

## Adibidea

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Espezializazioak

<div class="grid cards" markdown>

-   :material-account:{ .lg .middle } **PersonRef**

    ---

    Pertsona baten erreferentzia (`oid` + `id`).

    [:octicons-arrow-right-24: Ikusi eredua](./person-ref.md)

-   :material-domain:{ .lg .middle } **OrgAdminRef**

    ---

    Administrazio baten erreferentzia (`oid` + `id` + `dir3Id`).

    [:octicons-arrow-right-24: Ikusi eredua](./org-admin-ref.md)

-   :material-tag-outline:{ .lg .middle } **DataTypeRef**

    ---

    Datu mota baten erreferentzia (`oid` + `id`).

    [:octicons-arrow-right-24: Ikusi eredua](./data-type-ref.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
