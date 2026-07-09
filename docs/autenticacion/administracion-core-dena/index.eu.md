# :material-arrow-right-bold: Administrazioa → DENA CORE

> **Bertsioa:** `v{{ dena.version }}` · **Data:** {{ dena.date }}

---

## Zer da?

Administrazio batek DENAri eskaera bat egiten dionean, OAuth 2.0 token bat sartu behar du `Authorization: Bearer <token>` goiburuan.

``` mermaid
sequenceDiagram
    participant Admin as Administrazioa
    participant IDP as Keycloak DENA
    participant DENA as DENA CORE

    Admin->>IDP: POST /token (client_credentials)
    IDP-->>Admin: access_token
    Admin->>DENA: POST /api/... + Bearer token
    DENA-->>Admin: 200 OK
```

---

## Konfigurazioa

!!! info "Beharrezko kredentzialak"

    DENAk administrazioari honako hauek emango dizkio:

    - `client_id` — Bezeroaren identifikadorea
    - `client_secret` — Bezeroaren sekretua
    - Token endpoint-aren URLa

    Administrazioak kredentzial hauek bere sisteman konfiguratu behar ditu eta token-a **client_credentials** fluxu baten bidez sortu behar du.

---

## Dokumentazioa

| Dokumentua | Edukia |
|---|---|
| [:octicons-arrow-right-24: Get-token endpoint-a](./endpoint-get-token.md) | OAuth token-a lortzeko endpoint-a |

---

!!! tip "Postman"

    Postman bilduma eta environment-a eskuragarri hemen: [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
