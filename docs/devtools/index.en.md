# :material-wrench: DevTools

Development tools that facilitate testing and integration with DENA for public administrations.

---

## Available tools

<div class="grid cards" markdown>

-   :material-web:{ .lg .middle } **DENA DevTools Services**

    ---

    Web tool (Postman-like) for HTTP endpoint testing from DENA infrastructure.

    [:octicons-arrow-right-24: See below](#dena-devtools-services)

-   :material-connection:{ .lg .middle } **DENA Admin Connection Test**

    ---

    Deployable component to validate bidirectional connectivity Admin ↔ DENA.

    [:octicons-arrow-right-24: Repository]({{ repos.conx_test_tree }})

</div>

---

## DENA DevTools Services

### Available environments

=== ":material-earth: Internet"

    | Environment | URL |
    |---|---|
    | **PRE** | [https://api-batera.pre.dena.eus/devtools-services/](https://api-batera.pre.dena.eus/devtools-services/) |
    | **PRO** | [https://api-batera.pro.dena.eus/devtools-services/](https://api-batera.pro.dena.eus/devtools-services/) *(coming soon)* |

=== ":material-lan: Euskalsarea"

    | Environment | URL |
    |---|---|
    | **PRE** | [https://api-batera.pre.batera.euskalsarea.eus/devtools-services/](https://api-batera.pre.batera.euskalsarea.eus/devtools-services/) |
    | **PRO** | [https://api-batera.pro.batera.euskalsarea.eus/devtools-services/](https://api-batera.pro.batera.euskalsarea.eus/devtools-services/) *(coming soon)* |

### Features

- :material-swap-horizontal: **HTTP Methods** — GET, POST, PUT, DELETE, PATCH
- :material-shield-key: **Authentication** — Bearer Token, Basic Auth, API Key
- :material-code-json: **Body** — JSON, Form Data, URL Encoded, Raw Text
- :material-format-list-bulleted: **Headers** — Full header configuration
- :material-magnify: **Query parameters** — Add parameters automatically
- :material-monitor-dashboard: **Detailed response** — Status code, headers and body
- :material-certificate: **Flexible SSL/TLS** — Support for untrusted certificates
- :material-text-box-search: **Logging** — Full traceability with UUID per request

### REST API

!!! example "POST /api/devtools/test-endpoint"

    === "Request"

        ```json
        {
          "method": "POST",
          "url": "https://api.administration.com/endpoint",
          "headers": {
            "Content-Type": "application/json",
            "Authorization": "Bearer <token>"
          },
          "body": "{\"key\": \"value\"}"
        }
        ```

    === "Response"

        ```json
        {
          "statusCode": 200,
          "responseBody": "...",
          "responseHeaders": {...},
          "success": true
        }
        ```

### Use cases

| Case | Description |
|---|---|
| Testing from DENA | Test connectivity from Tanzú to administrations |
| Endpoint validation | Verify that administration services are accessible |
| Debugging | Diagnose connectivity or format issues |
| Authentication testing | Validate tokens, certificates and credentials |

---

## DENA Admin Connection Test

!!! info ""

    **Repository:** [DENA-Euskadi/dena-admin-conx-test]({{ repos.conx_test_tree }})

    Deployable tool in the administration's infrastructure to validate bidirectional connectivity.

    [:octicons-arrow-right-24: Full usage guide](../guia-inicio/probar-comunicaciones.md)

**Validations:**

- :material-check: Connectivity with DENA endpoints
- :material-check: OAuth2 authentication configuration
- :material-check: Request/response format and structure
- :material-check: Certificates and network configuration

---

## Requirements

| Requirement | Detail |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :material-shield-key: OAuth2 credentials | Provided by DENA |
| :material-lock: HTTPS connectivity | Towards DENA environments |

---

## Support

| Channel | Contact |
|---|---|
| :material-book-open-variant: Documentation | [This documentation](../index.md) |
| :material-bug: Issues | Report in the corresponding repository |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
