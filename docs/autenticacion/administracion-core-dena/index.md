# :material-arrow-right-bold: Administración → CORE DENA

> **Versión:** `v{{ dena.version }}` · **Fecha:** {{ dena.date }}

---

## ¿Qué es?

Cuando una administración realiza una petición a DENA, debe incluir un token OAuth 2.0 en la cabecera `Authorization: Bearer <token>`.

``` mermaid
sequenceDiagram
    participant Admin as Administración
    participant IDP as Keycloak DENA
    participant DENA as CORE DENA

    Admin->>IDP: POST /token (client_credentials)
    IDP-->>Admin: access_token
    Admin->>DENA: POST /api/... + Bearer token
    DENA-->>Admin: 200 OK
```

---

## Configuración

!!! info "Credenciales necesarias"

    DENA proveerá a la administración:

    - `client_id` — Identificador del cliente
    - `client_secret` — Secreto del cliente
    - URL del token endpoint

    La administración debe configurar estas credenciales en su sistema y generar el token mediante un flujo **client_credentials**.

---

## Documentación

| Documento | Contenido |
|---|---|
| [:octicons-arrow-right-24: Endpoint get-token](./endpoint-get-token.md) | Endpoint para la obtención del token OAuth |

---

!!! tip "Postman"

    Colección y environment Postman disponibles en [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
