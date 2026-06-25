# :material-key: Get DENA Token

## Endpoint

```
POST /realms/DenaAuthAdmins/protocol/openid-connect/token
Content-Type: application/x-www-form-urlencoded
```

---

## Request

=== "Parámetros"

    | Campo | Tipo | Obligatorio | Descripción |
    |---|---|:---:|---|
    | `grant_type` | `String` | :material-check: | `client_credentials` |
    | `client_id` | `String` | :material-check: | Id de cliente provisionado por DENA |
    | `client_secret` | `String` | :material-check: | Secreto de cliente provisionado por DENA |

=== "Ejemplo curl"

    ```bash
    curl -X POST \
      https://<keycloak-host>/realms/DenaAuthAdmins/protocol/openid-connect/token \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -d "grant_type=client_credentials&client_id=<your-client-id>&client_secret=<your-client-secret>"
    ```

=== "Ejemplo form body"

    ```
    grant_type=client_credentials
    client_id={provided client id}
    client_secret={provided client secret}
    ```

---

## Response

=== ":material-check-circle: Exitosa (HTTP 200)"

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

    | Campo | Descripción |
    |---|---|
    | `access_token` | Token JWT a incluir en la cabecera `Authorization: Bearer <token>` |
    | `expires_in` | Segundos hasta expiración (típicamente 300 = 5 min) |
    | `token_type` | Siempre `Bearer` |

=== ":material-close-circle: Error (HTTP 4xx)"

    ```json
    {
        "error": "invalid_client",
        "error_description": "Invalid client or Invalid client credentials"
    }
    ```

    | Error | Causa |
    |---|---|
    | `invalid_client` | `client_id` o `client_secret` incorrectos |
    | `unauthorized_client` | El cliente no tiene permiso para `client_credentials` |
    | `invalid_grant` | `grant_type` no válido |

---

## Uso del token

!!! example "Incluir en las llamadas a DENA"

    ```http
    POST /api/syncMetadata
    Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
    Content-Type: application/json
    ```

!!! warning "Renovación"

    El token expira tras `expires_in` segundos. Se recomienda renovarlo con un **leeway** de ~60 segundos antes de la expiración para evitar rechazos por latencia.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
