# :material-cellphone-link: Cliente ↔ CORE DENA

La autenticación entre la aplicación cliente y DENA utiliza OAuth 2.0. El token obtenido se incluye en las llamadas mediante la cabecera `Authorization: Bearer <token>`.

---

## Mecanismos de autenticación

=== ":material-shield-key: Giltza (primer acceso)"

    En el primer acceso, el usuario se autentica con **Giltza**. DENA valida el token de Giltza y genera un token OAuth propio para la comunicación.

    ![Flujo autenticación Giltza](../adjuntos/imagenes/login-giltza.png)

    ``` mermaid
    sequenceDiagram
        participant User as Usuario
        participant App as App Cliente
        participant Giltza as Giltza
        participant DENA as CORE DENA

        User->>App: Accede a la app
        App->>Giltza: Redirige a login
        Giltza-->>App: Token Giltza
        App->>DENA: Envía token Giltza
        DENA-->>App: Token OAuth DENA
    ```

=== ":material-fingerprint: WebAuthn (accesos posteriores)"

    Tras el primer acceso con Giltza, el usuario puede registrar credenciales WebAuthn para accesos futuros más rápidos.

    **Registro:**

    ![Flujo de registro WebAuthn](../adjuntos/imagenes/webauthn-register.png)

    **Login:**

    ![Flujo de autenticación WebAuthn](../adjuntos/imagenes/webauthn-login.png)

---

## Uso del token

!!! example "Incluir token en las llamadas"

    ```http
    GET /api/resource
    Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
    ```

    El token tiene una duración limitada (`expires_in`). La app debe renovarlo antes de que expire.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
