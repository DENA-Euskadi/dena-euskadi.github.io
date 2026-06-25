# :material-shield-lock: Autenticación

Mecanismos de autenticación entre los actores del ecosistema DENA.

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
    C[App Cliente] -->|OAuth2 / WebAuthn| D(CORE DENA)
    D -->|client_credentials| A[Administración]
    A -->|client_credentials| D
    
    style C fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px
    style D fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style A fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## Flujos disponibles

<div class="grid cards" markdown>

-   :material-cellphone-link:{ .lg .middle } **Cliente ↔ CORE DENA**

    ---

    Autenticación de la aplicación cliente contra DENA mediante Giltza o WebAuthn.

    [:octicons-arrow-right-24: Ver detalle](./client-core-dena.md)

-   :material-arrow-right-bold:{ .lg .middle } **Administración → CORE DENA**

    ---

    La administración obtiene un token OAuth2 para enviar peticiones a DENA (ej: Metadata-Sync).

    [:octicons-arrow-right-24: Ver detalle](./administracion-core-dena/index.md)

-   :material-arrow-left-bold:{ .lg .middle } **CORE DENA → Administración**

    ---

    DENA obtiene un token OAuth2 para consultar datos de la administración (ej: Data-Retrieve).

    [:octicons-arrow-right-24: Ver detalle](./core-dena-administracion/index.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
