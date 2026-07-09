# :material-shield-lock: Autentifikazioa

DENA ekosistemako aktoreen arteko autentifikazio mekanismoak.

---

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "14px"
    fontFamily: "Manrope, sans-serif"
---
graph LR
    C[Bezero App] -->|OAuth2 / WebAuthn| D(DENA CORE)
    D -->|client_credentials| A[Administrazioa]
    A -->|client_credentials| D
    
    style C fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px
    style D fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style A fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## Fluxu eskuragarriak

<div class="grid cards" markdown>

-   :material-cellphone-link:{ .lg .middle } **Bezeroa ↔ DENA CORE**

    ---

    Bezero aplikazioaren autentifikazioa DENAren aurka Giltza edo WebAuthn erabiliz.

    [:octicons-arrow-right-24: Xehetasunak ikusi](./client-core-dena.md)

-   :material-arrow-right-bold:{ .lg .middle } **Administrazioa → DENA CORE**

    ---

    Administrazioak OAuth2 token bat lortzen du DENAri eskaerak bidaltzeko (adib.: Metadata-Sync).

    [:octicons-arrow-right-24: Xehetasunak ikusi](./administracion-core-dena/index.md)

-   :material-arrow-left-bold:{ .lg .middle } **DENA CORE → Administrazioa**

    ---

    DENAk OAuth2 token bat lortzen du administrazioaren datuak kontsultatzeko (adib.: Data-Retrieve).

    [:octicons-arrow-right-24: Xehetasunak ikusi](./core-dena-administracion/index.md)

</div>

---

!!! tip "OAuth2 kredentzialak eta konfigurazioa"
    
    Kredentzialak (`client_id` + `client_secret`) eskatzeko edo autentifikazio arazoak konpontzeko, jarri harremanetan DENA taldearekin:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
