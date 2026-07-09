# :material-arrow-left-bold: DENA CORE → Administrazioa

DENAk administrazio bati dei bat egiten dionean autentifikazio estandarra **OAuth 2.0 client_credentials** da.

---

## Fluxua

``` mermaid
sequenceDiagram
    participant DENA as DENA CORE
    participant IDP as Administrazioaren IDP
    participant Admin as Administrazioaren API

    DENA->>IDP: POST /token (client_credentials)
    IDP-->>DENA: access_token
    DENA->>Admin: POST /api/retrieveData + Bearer token
    Admin-->>DENA: 200 OK + datuak
```

---

## Zer behar du administrazioak?

!!! info "DENAri eman"

    Administrazioak DENAri kredentzial hauek eman behar dizkio:

    - `client_id` — DENAren bezero identifikadorea
    - `client_secret` — Lotutako sekretua
    - **Token URLa** — Token-a lortzeko endpoint-a (adib.: Keycloak, Cognito, ADFS...)

    DENAk kredentzial hauek erabiliko ditu administrazioaren endpoint-era dei bakoitza egin aurretik token bat lortzeko.

---

## Edukia

| Dokumentua | Deskribapena |
|---|---|
| [:octicons-arrow-right-24: Eredua](./modelo.md) | Erabilitako datu eredua / token-ak |
| [:octicons-arrow-right-24: Zerbitzuak](./servicios.md) | Autentifikaziorako esposatu/kontsumitutako zerbitzuak |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
