# :material-account: PersonRef

## Deskribapena

DENA-n erregistratutako pertsona bati bere `oid` edo `id` bidez erreferentzia egiteko objektua (IFZ, AIZ, etab.).

!!! info "Gutxienez bat derrigorrez"

    `oid` **edo** `id` sartu behar da (edo biak).

---

## JSON atributuak

| Eremua | Mota | Derrigorrez | Deskribapena |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Pertsonaren barne-identifikatzailea |
| `id` | `String` | :material-close:* | Kanpo-identifikatzailea (IFZ, AIZ, etab.) |

---

## Adibidea

```json
{
    "id": "12345678A",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
