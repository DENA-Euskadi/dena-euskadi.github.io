# :material-tag: DataTypeRef

## Deskribapena

DENAk kudeatutako datu mota bati erreferentzia egiteko objektua (adib. Espedientea, Jakinarazpena, Ordainketa...).

!!! info "Gutxienez bat derrigorrez"

    `oid` **edo** `id` sartu behar da (edo biak). Biak sartzen badira, `oid`-k lehentasuna du.

---

## JSON atributuak

| Eremua | Mota | Derrigorrez | Deskribapena |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Datu motaren barne-identifikatzailea |
| `id` | `String` | :material-close:* | Datu motaren testu-identifikatzailea |

---

## Adibidea

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## `id`-ren balio estandarrak

| `id` | Datu mota |
|---|---|
| `RECORDS` | Espedienteak |
| `NOTICES` | Jakinarazpenak |
| `REGISTRY` | Erregistro ofizialak |
| `PAYMENTS` | Ordainketak |
| `SCHEDULE` | Hitzorduak |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
