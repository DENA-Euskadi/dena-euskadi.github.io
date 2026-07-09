# :material-upload: PERSON-SYNC — PUSH

DENAk modu proaktiboan jakinarazten dio administrazioari pertsona berri bat erregistratzen denean edo bere datuetan aldaketak gertatzen direnean.

---

## Fluxua

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA
    participant Admin as Administrazioa

    Note over DENA: Pertsona berria erregistratzen da / aldaketa gertatzen da
    DENA->>Admin: POST /api/person-push (aldaketaren xehetasunak)
    Admin-->>DENA: 200 OK
```

---

## Zer behar du administrazioak?

!!! info "Endpoint bat inplementatu"

    Administrazioak aldaketaren xehetasunak jasotzeko gai den REST endpoint bat eskaini behar du.
    DENAk endpoint hau deituko du pertsona baten alta edo aldaketa gertatzen den bakoitzean.

---

## Endpoint-aren kontratua

[:octicons-arrow-right-24: Endpoint Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) — Eskaera, erantzuna, JSON adibideak eta HTTP kodeak.

---

!!! tip "Noiz erabili Push"

    - Pertsonen aldaketei **denbora errealean** erantzun behar duzunean
    - Aldizkako fitxategietan (Pull) menpeko izan nahi ez duzunean
    - Zure sistemak datua berehala behar duenean Metadata-Sync bidez aldaketak jakinarazteko

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
