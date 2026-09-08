# :material-shield-lock: Seguridad y Autenticación

Esta sección explica cómo se protegen las comunicaciones en DENA y cómo se autentica cada parte del sistema. Primero los conceptos, luego la guía práctica.

---

## Objetivos de seguridad

La seguridad en DENA tiene cuatro objetivos:

1. **Origen legítimo de la petición:** Asegurar que el servicio solo es accedido por partes autorizadas y verificadas.
2. **Integridad de la petición:** Asegurar que los datos intercambiados no se alteran durante el tránsito.
3. **Confidencialidad de datos:** Asegurar que los datos permanecen cifrados e invisibles para terceros no autorizados.
4. **Prevenir ataques de replay:** Prevenir que terceros capturen y reproduzcan peticiones.

| Objetivo | HTTPS | JWT | Security Header |
|----------|:-----:|:---:|:---------------:|
| Origen legítimo | | :material-check: | :material-check: |
| Integridad | | :material-check: | :material-check: |
| Confidencialidad | :material-check: | :material-check: | |
| Anti-replay | | :material-check: | :material-check: |

---

## Áreas de autenticación

Hay cuatro áreas donde diferentes partes se autentican:

![Security Areas](../adjuntos/imagenes/arquitectura/security-areas.png)

| Área | Quién se autentica | Con quién | Mecanismo |
|------|-------------------|-----------|-----------|
| **1** | Persona | DENA-APP | GILTZA OAuth token |
| **2** | DENA-APP | DENA-CORE | DENA-IdP OAuth token |
| **3** | Administración | DENA-CORE | DENA-IdP OAuth token (client_credentials) |
| **4** | DENA-CORE | Administración | Lo que la admin prefiera (OAuth, X.509, usr/pwd...) |

!!! info "Para las administraciones"
    Las áreas que te afectan como administración son la **3** (tu admin se autentica en DENA) y la **4** (DENA se autentica en tu admin). Las áreas 1 y 2 son internas de la app de la persona.

---

## Flujo 1: Persona → DENA-APP (GILTZA)

![Auth GILTZA](../adjuntos/imagenes/arquitectura/auth-giltza-flow.png)

1. La persona entra en DENA-APP y es redirigida a la página de login de **GILTZA** (web-view)
2. La persona hace login
3. GILTZA devuelve un OAuth token

---

## Flujo 2: DENA-APP → DENA-CORE (DENA-IdP)

![Auth DENA-APP CORE](../adjuntos/imagenes/arquitectura/auth-dena-app-core.png)

DENA-APP se identifica con DENA-CORE usando el GILTZA token. A cambio recibe un **DENA-IdP OAuth token** que usará en todas las llamadas posteriores (init, sync SRMD, retrieve data...).

!!! warning "Expiración"
    El token DENA-IdP **expira**. DENA-APP debe refrescarlo periódicamente.

---

## Flujo 3: Tu Administración → DENA-CORE (client_credentials)

![Auth Admin CORE](../adjuntos/imagenes/arquitectura/auth-admin-core.png)

Tu administración se autentica en DENA usando **client_credentials** OAuth2:

1. Solicitas `client_id` + `client_secret` al equipo DENA
2. Envías las credenciales al endpoint de token de DENA-IdP
3. Recibes un OAuth token
4. Usas ese token en cada llamada a DENA-CORE (enviar SRMD, consultar personas, etc.)

**Este es el flujo que necesitas implementar para:**

- Enviar Metadata-Sync
- Consultar datos de personas (Person-Sync Pull)
- Cualquier llamada de tu admin hacia DENA

[:octicons-arrow-right-24: Guía práctica de implementación](../autenticacion/administracion-core-dena/index.md)

---

## Flujo 4: DENA-CORE → Tu Administración

![Auth CORE Admin](../adjuntos/imagenes/arquitectura/auth-core-admin.png)

Cuando DENA-CORE llama a tu admin (para Data-Retrieve), debe autenticarse. DENA **NO impone** un mecanismo; cada admin elige el suyo:

- **OAuth**: tu admin proporciona client credentials a DENA
- **Certificados X.509**: autenticación mutua TLS
- **User/password**: credenciales básicas
- Otros

Tu admin proporciona al equipo DENA las credenciales necesarias para el mecanismo elegido.

[:octicons-arrow-right-24: Guía práctica de implementación](../autenticacion/core-dena-administracion/index.md)

---

## Guías prácticas de implementación

Las siguientes páginas contienen los detalles técnicos de implementación:

| Flujo | Página |
|-------|--------|
| Tu admin llama a DENA | [:octicons-arrow-right-24: Endpoint get-token + ejemplos](../autenticacion/administracion-core-dena/index.md) |
| DENA llama a tu admin | [:octicons-arrow-right-24: Configuración + modelo](../autenticacion/core-dena-administracion/index.md) |

---

**Siguiente:** [:octicons-arrow-right-24: Operativas](../operativas/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
