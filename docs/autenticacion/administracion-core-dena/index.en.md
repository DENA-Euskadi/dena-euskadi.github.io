# :material-arrow-right-bold: Administration → DENA CORE

> **Version:** `v{{ dena.version }}` · **Date:** {{ dena.date }}

---

## What is it?

When an administration makes a request to DENA, it must include an OAuth 2.0 token in the `Authorization: Bearer <token>` header.

``` mermaid
sequenceDiagram
    participant Admin as Administration
    participant IDP as Keycloak DENA
    participant DENA as DENA CORE

    Admin->>IDP: POST /token (client_credentials)
    IDP-->>Admin: access_token
    Admin->>DENA: POST /api/... + Bearer token
    DENA-->>Admin: 200 OK
```

---

## Configuration

!!! info "Required credentials"

    DENA will provide the administration with:

    - `client_id` — Client identifier
    - `client_secret` — Client secret
    - Token endpoint URL

    The administration must configure these credentials in its system and generate the token using a **client_credentials** flow.

---

## Documentation

| Document | Content |
|---|---|
| [:octicons-arrow-right-24: Get-token endpoint](./endpoint-get-token.md) | Endpoint for obtaining the OAuth token |

---

!!! tip "Postman"

    Postman collection and environment available at [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
