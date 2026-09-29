# :material-table-check: Bateragarritasun-matrizea

Zein endpoint dira nahitaezkoak edo aukerakoak zure integrazio-motaren arabera?

---

## Erabilera-kasuaren arabera

| Gaitasuna | Data-Retrieve | Metadata-Sync | Person-Sync Pull | Person-Sync Push |
|---|:---:|:---:|:---:|:---:|
| **DENAk nire administrazioaren datuak kontsultatzen ditu** | :material-check-bold:{ title="Nahitaezkoa" } | :material-minus:{ title="Aukerakoa" } | :material-minus:{ title="Aukerakoa" } | :material-minus:{ title="Aukerakoa" } |
| **DENAri jakinarazten diot aldaketak daudenean** | :material-close:{ title="Ez da aplikagarria" } | :material-check-bold:{ title="Nahitaezkoa" } | :material-minus:{ title="Aukerakoa" } | :material-minus:{ title="Aukerakoa" } |
| **Pertsonen zerrenda mantentzen dut (batch)** | :material-close:{ title="Ez da aplikagarria" } | :material-close:{ title="Ez da aplikagarria" } | :material-check-bold:{ title="Nahitaezkoa" } | :material-close:{ title="Ez da aplikagarria" } |
| **Pertsonak denbora errealean jasotzen ditut** | :material-close:{ title="Ez da aplikagarria" } | :material-close:{ title="Ez da aplikagarria" } | :material-close:{ title="Ez da aplikagarria" } | :material-check-bold:{ title="Nahitaezkoa" } |
| **Integrazio osoa** | :material-check-bold:{ title="Nahitaezkoa" } | :material-check-bold:{ title="Nahitaezkoa" } | :material-check-bold:{ title="Nahitaezkoa" } | :material-minus:{ title="Aukerakoa" } |

!!! note "Legenda"

    - :material-check-bold: — Kasu honetarako nahitaezkoa
    - :material-minus: — Aukerakoa (gomendatua baina ez beharrezkoa)
    - :material-close: — Ez da aplikagarria

---

## Rolaren arabera (nork inplementatzen du zer)

| Endpoint-a | Nork inplementatzen du | Nork deitzen du |
|---|---|---|
| `POST` Data-Retrieve (URL konfiguragarria; adib. `/api/retrieveData`) | :material-domain: Administrazioa | :material-swap-horizontal: DENA |
| `POST` Person Push (URL konfiguragarria) | :material-domain: Administrazioa | :material-swap-horizontal: DENA |
| `POST /api/admin/interop/sync/metadata` (Metadata-Sync) | :material-swap-horizontal: DENA | :material-domain: Administrazioa |
| `POST /api/admin/persons/sync/...` (Person-Sync Pull: bespokes/pregens) | :material-swap-horizontal: DENA | :material-domain: Administrazioa |

> Administrazioak **inplementatzen dituen** endpoint-ak (Data-Retrieve eta Person Push) administrazioak DENAn konfiguratzen duen URLan erakusten dira; erakutsitako bideak adibideak/erreferentzia dira. DENAk **inplementatzen dituen** endpoint-ek (Metadata-Sync eta Person-Sync Pull) bide finkoa dute.

---

## Datu-motaren arabera

| dataTypeId | Itzulitako objektua | Dokumentazioa |
|---|---|---|
| `RECORDS` | Espedienteak | [expediente.md](../semantica/data-retrieve/data/expediente.md) |
| `NOTICES` | Jakinarazpenak | [notificacion.md](../semantica/data-retrieve/data/notificacion.md) |
| `REGISTER` | Erregistro ofizialak | [registro-oficial.md](../semantica/data-retrieve/data/registro-oficial.md) |
| `PAYMENTS` | Ordainketak | [pago.md](../semantica/data-retrieve/data/pago.md) |
| `SCHEDULE` | Hitzorduak | [cita.md](../semantica/data-retrieve/data/cita.md) |

!!! tip "Ez duzu mota guztiak inplementatu behar"

    Inplementatu soilik zure administrazioak kudeatzen dituen datu-motak.
    Ez duzun mota bat eskatzen badizute, itzuli `dataItems: []` `code: "OK"`-rekin.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
