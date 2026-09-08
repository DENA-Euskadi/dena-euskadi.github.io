# :material-cogs: Operatibak

Atal honek zure administrazioak DENArekin integratzeko **inplementa ditzakeen eragiketak** deskribatzen ditu. Operatiba bakoitza lehenik kontzeptualki azaltzen da eta gero inplementazio teknikoaren xehetasunarekin.

---

## Gomendatutako fluxua

1. **Hasi Data-Retrieve-rekin** — Oinarrizko operatiba da. Hau gabe, DENAk ezin ditu zure adminaren datuak erakutsi.
2. **Gero Metadata-Sync** — DENAk jakin dezan noiz dauden datu berriak etengabe kontsultatu beharrik gabe.
3. **Azkenik Person-Sync** — DENAn zer pertsona dauden jakiteko eta haientzat soilik jakinarazpenak bidaltzeko.

---

## Operatiba erabilgarriak

<div class="grid cards" markdown>

-   :material-database-arrow-right:{ .lg .middle } **Data-Retrieve (Datuak Zerbitzatu)**

    ---

    DENAk zure administrazioari deitzen dio pertsona baten datuak lortzeko.

    [:octicons-arrow-right-24: Xehetasuna ikusi](./data-retrieve.md)

-   :material-bell-ring:{ .lg .middle } **Metadata-Sync (Aldaketak Jakinarazi)**

    ---

    Zure administrazioak DENAri jakinarazpenak bidaltzen dizkio datu berriak edo eguneratuak daudela.

    [:octicons-arrow-right-24: Xehetasuna ikusi](./metadata-sync.md)

-   :material-account-sync:{ .lg .middle } **Person-Sync (Pertsonak Sinkronizatu)**

    ---

    DENAn inskribatutako pertsonen zerrenda sinkronizatuta mantendu.

    [:octicons-arrow-right-24: Xehetasuna ikusi](./person-sync.md)

</div>

---

**Hurrengoa:** [:octicons-arrow-right-24: Semantika (zehaztapen teknikoa)](../semantica/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
