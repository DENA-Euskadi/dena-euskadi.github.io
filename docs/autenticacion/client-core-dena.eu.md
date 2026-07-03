# :material-cellphone-link: Bezeroa ↔ DENA CORE

Bezero aplikazioaren eta DENA-ren arteko autentifikazioak OAuth 2.0 erabiltzen du. Lortutako token-a deietan sartzen da `Authorization: Bearer <token>` goiburuaren bidez.

---

## Autentifikazio mekanismoak

=== ":material-shield-key: Giltza (lehen sarbidea)"

    Lehen sarbidean, erabiltzaileak **Giltza**-rekin autentifikatzen du. DENA-k Giltza token-a balioztatzen du eta bere OAuth token propioa sortzen du komunikaziorako.

    ![Giltza autentifikazio fluxua](../adjuntos/imagenes/login-giltza.png)

    ``` mermaid
    sequenceDiagram
        participant User as Erabiltzailea
        participant App as Bezero App
        participant Giltza as Giltza
        participant DENA as DENA CORE

        User->>App: App-era sartzen da
        App->>Giltza: Login-era birbideratzen du
        Giltza-->>App: Giltza Token-a
        App->>DENA: Giltza token-a bidaltzen du
        DENA-->>App: DENA OAuth Token-a
    ```

=== ":material-fingerprint: WebAuthn (ondorengo sarbideak)"

    Giltza-rekin lehen sarbidearen ostean, erabiltzaileak WebAuthn kredentzialak erregistra ditzake etorkizuneko sarbide azkarragoetarako.

    **Erregistroa:**

    ![WebAuthn erregistro fluxua](../adjuntos/imagenes/webauthn-register.png)

    **Login-a:**

    ![WebAuthn autentifikazio fluxua](../adjuntos/imagenes/webauthn-login.png)

---

## Token-aren erabilera

!!! example "Token-a deietan sartu"

    ```http
    GET /api/resource
    Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
    ```

    Token-ak iraupen mugatua du (`expires_in`). App-ak iraungitu aurretik berritu behar du.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
