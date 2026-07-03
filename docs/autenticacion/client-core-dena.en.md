# :material-cellphone-link: Client ↔ DENA CORE

Authentication between the client application and DENA uses OAuth 2.0. The obtained token is included in calls via the `Authorization: Bearer <token>` header.

---

## Authentication mechanisms

=== ":material-shield-key: Giltza (first access)"

    On first access, the user authenticates with **Giltza**. DENA validates the Giltza token and generates its own OAuth token for communication.

    ![Giltza authentication flow](../adjuntos/imagenes/login-giltza.png)

    ``` mermaid
    sequenceDiagram
        participant User as User
        participant App as Client App
        participant Giltza as Giltza
        participant DENA as DENA CORE

        User->>App: Accesses the app
        App->>Giltza: Redirects to login
        Giltza-->>App: Giltza Token
        App->>DENA: Sends Giltza token
        DENA-->>App: DENA OAuth Token
    ```

=== ":material-fingerprint: WebAuthn (subsequent accesses)"

    After the first access with Giltza, the user can register WebAuthn credentials for faster future accesses.

    **Registration:**

    ![WebAuthn registration flow](../adjuntos/imagenes/webauthn-register.png)

    **Login:**

    ![WebAuthn authentication flow](../adjuntos/imagenes/webauthn-login.png)

---

## Token usage

!!! example "Include token in calls"

    ```http
    GET /api/resource
    Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
    ```

    The token has a limited duration (`expires_in`). The app must renew it before it expires.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
