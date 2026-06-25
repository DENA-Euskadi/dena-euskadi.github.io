# :material-wrench: DevTools

Herramientas de desarrollo que facilitan las pruebas e integración con DENA para administraciones públicas.

---

## Herramientas disponibles

<div class="grid cards" markdown>

-   :material-web:{ .lg .middle } **DENA DevTools Services**

    ---

    Herramienta web (tipo Postman) para testing de endpoints HTTP desde la infraestructura DENA.

    [:octicons-arrow-right-24: Ver más abajo](#dena-devtools-services)

-   :material-connection:{ .lg .middle } **DENA Admin Connection Test**

    ---

    Componente desplegable para validar la conectividad bidireccional Admin ↔ DENA.

    [:octicons-arrow-right-24: Repositorio]({{ repos.conx_test_tree }})

</div>

---

## DENA DevTools Services

### Entornos disponibles

=== ":material-earth: Internet"

    | Entorno | URL |
    |---|---|
    | **PRE** | [https://api-batera.pre.dena.eus/devtools-services/](https://api-batera.pre.dena.eus/devtools-services/) |
    | **PRO** | [https://api-batera.pro.dena.eus/devtools-services/](https://api-batera.pro.dena.eus/devtools-services/) *(próximamente)* |

=== ":material-lan: Euskalsarea"

    | Entorno | URL |
    |---|---|
    | **PRE** | [https://api-batera.pre.batera.euskalsarea.eus/devtools-services/](https://api-batera.pre.batera.euskalsarea.eus/devtools-services/) |
    | **PRO** | [https://api-batera.pro.batera.euskalsarea.eus/devtools-services/](https://api-batera.pro.batera.euskalsarea.eus/devtools-services/) *(próximamente)* |

### Funcionalidades

- :material-swap-horizontal: **Métodos HTTP** — GET, POST, PUT, DELETE, PATCH
- :material-shield-key: **Autenticación** — Bearer Token, Basic Auth, API Key
- :material-code-json: **Body** — JSON, Form Data, URL Encoded, Raw Text
- :material-format-list-bulleted: **Headers** — Configuración completa de headers
- :material-magnify: **Query parameters** — Añadir parámetros automáticamente
- :material-monitor-dashboard: **Respuesta detallada** — Status code, headers y body
- :material-certificate: **SSL/TLS flexible** — Soporte para certificados no confiables
- :material-text-box-search: **Logging** — Trazabilidad completa con UUID por request

### API REST

!!! example "POST /api/devtools/test-endpoint"

    === "Request"

        ```json
        {
          "method": "POST",
          "url": "https://api.administracion.com/endpoint",
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

### Casos de uso

| Caso | Descripción |
|---|---|
| Testing desde DENA | Probar conectividad desde Tanzú hacia administraciones |
| Validación de endpoints | Verificar que los servicios de administración son accesibles |
| Debugging | Diagnosticar problemas de conectividad o formato |
| Pruebas de autenticación | Validar tokens, certificados y credenciales |

---

## DENA Admin Connection Test

!!! info ""

    **Repositorio:** [DENA-Euskadi/dena-admin-conx-test]({{ repos.conx_test_tree }})

    Herramienta desplegable en la infraestructura de la administración para validar conectividad bidireccional.

    [:octicons-arrow-right-24: Guía completa de uso](../guia-inicio/probar-comunicaciones.md)

**Validaciones:**

- :material-check: Conectividad con los endpoints de DENA
- :material-check: Configuración de autenticación OAuth2
- :material-check: Formato y estructura de requests/responses
- :material-check: Certificados y configuración de red

---

## Requisitos

| Requisito | Detalle |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :material-shield-key: Credenciales OAuth2 | Proporcionadas por DENA |
| :material-lock: Conectividad HTTPS | Hacia los entornos de DENA |

---

## Soporte

| Canal | Contacto |
|---|---|
| :material-book-open-variant: Documentación | [Esta documentación](../index.md) |
| :material-bug: Issues | Reportar en el repositorio correspondiente |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
