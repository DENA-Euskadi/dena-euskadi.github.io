# :material-arrow-left-bold: DENA CORE → Administration

The standard authentication when DENA makes a call to an administration is **OAuth 2.0 client_credentials**.

---

## Flow

``` mermaid
sequenceDiagram
    participant DENA as DENA CORE
    participant IDP as Administration IDP
    participant Admin as Administration API

    DENA->>IDP: POST /token (client_credentials)
    IDP-->>DENA: access_token
    DENA->>Admin: POST /api/retrieveData + Bearer token
    Admin-->>DENA: 200 OK + data
```

---

## What does the administration need?

!!! info "Provide to DENA"

    The administration must provide DENA with the following credentials:

    - `client_id` — Client identifier for DENA
    - `client_secret` — Associated secret
    - **Token URL** — Endpoint to obtain the token (e.g.: Keycloak, Cognito, ADFS...)

    DENA will use these credentials to obtain a token before each call to the administration's endpoint.

---

## Content

| Document | Description |
|---|---|
| [:octicons-arrow-right-24: Model](./modelo.md) | Data model / tokens used |
| [:octicons-arrow-right-24: Services](./servicios.md) | Services exposed/consumed for authentication |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
