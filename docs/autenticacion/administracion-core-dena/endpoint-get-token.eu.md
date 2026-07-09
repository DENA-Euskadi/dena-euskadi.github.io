# :material-key: DENA Token-a Lortu

## Endpoint-a

```
POST /realms/DenaAuthAdmins/protocol/openid-connect/token
Content-Type: application/x-www-form-urlencoded
```

---

## Eskaera

=== "Parametroak"

    | Eremua | Mota | Derrigorrezkoa | Deskribapena |
    |---|---|:---:|---|
    | `grant_type` | `String` | :material-check: | `client_credentials` |
    | `client_id` | `String` | :material-check: | DENAk hornitu duen bezero identifikadorea |
    | `client_secret` | `String` | :material-check: | DENAk hornitu duen bezero sekretua |

=== "curl adibidea"

    ```bash
    curl -X POST \
      https://<keycloak-host>/realms/DenaAuthAdmins/protocol/openid-connect/token \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -d "grant_type=client_credentials&client_id=<your-client-id>&client_secret=<your-client-secret>"
    ```

=== "Form body adibidea"

    ```
    grant_type=client_credentials
    client_id={emandako client id}
    client_secret={emandako client secret}
    ```

---

## Erantzuna

=== ":material-check-circle: Arrakastatsua (HTTP 200)"

    ```json
    {
        "access_token": "{access token}",
        "expires_in": 300,
        "refresh_expires_in": 0,
        "token_type": "Bearer",
        "not-before-policy": 0,
        "scope": "email profile"
    }
    ```

    | Eremua | Deskribapena |
    |---|---|
    | `access_token` | `Authorization: Bearer <token>` goiburuan sartu beharreko JWT token-a |
    | `expires_in` | Iraungitzera arteko segundoak (normalean 300 = 5 min) |
    | `token_type` | Beti `Bearer` |

=== ":material-close-circle: Errorea (HTTP 4xx)"

    ```json
    {
        "error": "invalid_client",
        "error_description": "Invalid client or Invalid client credentials"
    }
    ```

    | Errorea | Kausa |
    |---|---|
    | `invalid_client` | `client_id` edo `client_secret` okerrak |
    | `unauthorized_client` | Bezeroak ez du baimenik `client_credentials` fluxurako |
    | `invalid_grant` | `grant_type` ez baliozkoa |

---

## Token-aren erabilera

!!! example "DENAri egindako deietan sartu"

    ```http
    POST /api/syncMetadata
    Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
    Content-Type: application/json
    ```

!!! warning "Berritzea"

    Token-a `expires_in` segundoren ostean iraungitzen da. Gomendagarria da iraungitze aurretik ~60 segundoko **leeway**-arekin berritzea, latentzia ondoriozko bazterketak ekiditeko.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
