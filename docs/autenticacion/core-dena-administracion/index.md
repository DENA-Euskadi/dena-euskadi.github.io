# :material-arrow-left-bold: CORE DENA → Administración

La autenticación estándar cuando DENA realiza una llamada a una administración es **OAuth 2.0 client_credentials**.

---

## Flujo

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA
    participant IDP as IDP Administración
    participant Admin as API Administración

    DENA->>IDP: POST /token (client_credentials)
    IDP-->>DENA: access_token
    DENA->>Admin: POST /api/retrieveData + Bearer token
    Admin-->>DENA: 200 OK + datos
```

---

## ¿Qué necesita la administración?

!!! info "Proveer a DENA"

    La administración debe proveer a DENA las siguientes credenciales:

    - `client_id` — Identificador del cliente para DENA
    - `client_secret` — Secreto asociado
    - **Token URL** — Endpoint para obtener el token (ej: Keycloak, Cognito, ADFS...)

    DENA usará estas credenciales para obtener un token antes de cada llamada al endpoint de la administración.

---

## Contenido

| Documento | Descripción |
|---|---|
| [:octicons-arrow-right-24: Modelo](./modelo.md) | Modelo de datos / tokens utilizado |
| [:octicons-arrow-right-24: Servicios](./servicios.md) | Servicios expuestos/consumidos para autenticación |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
