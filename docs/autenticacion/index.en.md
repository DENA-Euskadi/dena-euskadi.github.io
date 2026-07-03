# :material-shield-lock: Authentication

Authentication mechanisms between the actors of the DENA ecosystem.

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
    C[Client App] -->|OAuth2 / WebAuthn| D(DENA CORE)
    D -->|client_credentials| A[Administration]
    A -->|client_credentials| D
    
    style C fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px
    style D fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style A fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## Available flows

<div class="grid cards" markdown>

-   :material-cellphone-link:{ .lg .middle } **Client ↔ DENA CORE**

    ---

    Client application authentication against DENA using Giltza or WebAuthn.

    [:octicons-arrow-right-24: View details](./client-core-dena.md)

-   :material-arrow-right-bold:{ .lg .middle } **Administration → DENA CORE**

    ---

    The administration obtains an OAuth2 token to send requests to DENA (e.g.: Metadata-Sync).

    [:octicons-arrow-right-24: View details](./administracion-core-dena/index.md)

-   :material-arrow-left-bold:{ .lg .middle } **DENA CORE → Administration**

    ---

    DENA obtains an OAuth2 token to query data from the administration (e.g.: Data-Retrieve).

    [:octicons-arrow-right-24: View details](./core-dena-administracion/index.md)

</div>

---

!!! tip "OAuth2 credentials and configuration"
    
    To request credentials (`client_id` + `client_secret`) or resolve authentication issues, contact the DENA team:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
