# :material-key: Get DENA Token

## Endpoint

```
POST /realms/DenaAuthAdmins/protocol/openid-connect/token
Content-Type: application/x-www-form-urlencoded
```

---

## Request

=== "Parameters"

    | Field | Type | Required | Description |
    |---|---|:---:|---|
    | `grant_type` | `String` | :material-check: | `client_credentials` |
    | `client_id` | `String` | :material-check: | Client ID provisioned by DENA |
    | `client_secret` | `String` | :material-check: | Client secret provisioned by DENA |

=== "curl example"

    ```bash
    curl -X POST \
      https://<keycloak-host>/realms/DenaAuthAdmins/protocol/openid-connect/token \
      -H "Content-Type: application/x-www-form-urlencoded" \
      -d "grant_type=client_credentials&client_id=<your-client-id>&client_secret=<your-client-secret>"
    ```

=== "Form body example"

    ```
    grant_type=client_credentials
    client_id={provided client id}
    client_secret={provided client secret}
    ```

---

## Response

=== ":material-check-circle: Success (HTTP 200)"

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

    | Field | Description |
    |---|---|
    | `access_token` | JWT token to include in the `Authorization: Bearer <token>` header |
    | `expires_in` | Seconds until expiration (typically 300 = 5 min) |
    | `token_type` | Always `Bearer` |

=== ":material-close-circle: Error (HTTP 4xx)"

    ```json
    {
        "error": "invalid_client",
        "error_description": "Invalid client or Invalid client credentials"
    }
    ```

    | Error | Cause |
    |---|---|
    | `invalid_client` | Incorrect `client_id` or `client_secret` |
    | `unauthorized_client` | Client does not have permission for `client_credentials` |
    | `invalid_grant` | Invalid `grant_type` |

---

## Token usage

!!! example "Include in calls to DENA"

    ```http
    POST /api/syncMetadata
    Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
    Content-Type: application/json
    ```

!!! warning "Renewal"

    The token expires after `expires_in` seconds. It is recommended to renew it with a **leeway** of ~60 seconds before expiration to avoid rejections due to latency.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
