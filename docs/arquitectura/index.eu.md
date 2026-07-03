# :material-cube-outline: Arkitektura

## Ikuspegi Orokorra

DENA Interop administrazio ezberdinekin elkarreragingarritasuna ahalbidetzen duen sistema da, pertsona erabiltzaileei, bezero aplikazioaren bidez, administrazioek haiei buruz kudeatzen dituzten datuetara sarbidea emateko.

![Arkitektura diagrama](../adjuntos/imagenes/DENA_Architecture.png)

---

## Modulu nagusiak

<div class="grid cards" markdown>

-   :material-sync:{ .lg .middle } **Metadata Sync**

    ---

    Administrazioen jakinarazpenak jasotzen ditu pertsona baten datuetan aldaketak daudenean. DENA-k azken eguneratze-data soilik gordetzen du pertsona + datu mota + administrazio konbinazio bakoitzeko.

    [:octicons-arrow-right-24: Metadata-Sync Semantika](../semantica/metadata-sync/index.md)

-   :material-database-arrow-right:{ .lg .middle } **Data Retrieve**

    ---

    Bezero aplikazioak aldaketak detektatzen dituenean, administrazioari (DENA bidez) eguneratutako datuak deskargatzeko eskatzen dio. Administrazioak endpoint estandar bat esposatzen du.

    [:octicons-arrow-right-24: Data-Retrieve Semantika](../semantica/data-retrieve/index.md)

-   :material-account-sync:{ .lg .middle } **Person Sync**

    ---

    DENA-n erregistratutako pertsonen zerrenda administrazioekin sinkronizatzen du. Bi mekanismo: **Pull** (administrazioak kontsultatzen du) eta **Push** (DENA-k jakinarazten du).

    [:octicons-arrow-right-24: Person-Sync Semantika](../semantica/person-sync/index.md)

</div>

---

## Fluxu orokorra

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "13px"
    fontFamily: "Manrope, sans-serif"
    actorBkg: "#f5d836"
    actorBorder: "#1a4d1f"
    actorTextColor: "#1a4d1f"
    activationBkgColor: "#70d680"
    activationBorderColor: "#1a4d1f"
    sequenceNumberColor: "#1a4d1f"
---
sequenceDiagram
    participant App as Bezero App
    participant DENA as DENA CORE
    participant Admin as Administrazioa

    Note over Admin,DENA: 1. Metadata Sync
    Admin->>DENA: POST /syncMetadata (aldaketak daude)
    DENA-->>Admin: 200 OK

    Note over App,DENA: 2. App-ak aldaketak detektatzen ditu
    App->>DENA: Metadatuak kontsultatu
    DENA-->>App: Datu berriak daude X Admin-en

    Note over DENA,Admin: 3. Data Retrieve
    DENA->>Admin: POST /retrieveData (pertsona + mota)
    Admin-->>DENA: 200 OK + datuak
    DENA-->>App: Eguneratutako datuak
```

---

## Konektore estandarra vs. ez-estandarra

!!! tip "Endpoint estandarra"

    Zure administrazioak REST endpoint estandarra inplementa badezake (`POST /api/retrieveData`), integrazioa zuzena eta azkarra da.

    [:octicons-arrow-right-24: Inplementazio gida](../semantica/data-retrieve/guia-implementacion.md)

!!! info "Konektore pertsonalizatua"

    Estandarra inplementatzea posible ez bada, DENA-tik administrazioak eskainitako bitartekoetara (SOAP, fitxategiak, etab.) egokitutako konektore espezifiko bat garatuko da.

---

## Dokumentazio erlazionatua

- [Diagramak (draw.io)](./diagramas.md)
- [Beste dokumentazioa](./otra-documentacion.md)
- [ADRak (Architecture Decision Records)]({{ repos.docs_main }}/ArchitectureDecisionRecords)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
