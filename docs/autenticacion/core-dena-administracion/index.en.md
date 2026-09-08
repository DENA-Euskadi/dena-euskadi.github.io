# :material-arrow-left-bold: DENA Calls Your System

When DENA needs to call your administration (for Data-Retrieve, for example), it must authenticate against your system. The **connector** of your administration is the component that manages this authentication.

---

## Fundamental principle

!!! info "The connector adapts to you"
    Each administration can offer whichever security mechanism it prefers. The **connector** — developed and maintained by the DENA team — is responsible for authenticating against your system using the mechanism your administration exposes.
    
    Your administration **does not need to implement anything special for DENA**. You only need to tell the DENA team how to authenticate.

---

## Standard flow: OAuth2 client_credentials

The recommended and most common mechanism is **OAuth2 `client_credentials`**. If your administration has an Identity Provider (Keycloak, ADFS, Cognito, Azure AD...), this is the flow:

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (Connector)
    participant IDP as Your administration's IDP
    participant Admin as Your REST endpoint

    DENA->>IDP: POST /token (client_credentials)
    Note right of IDP: Validates client_id + client_secret
    IDP-->>DENA: access_token (JWT)
    DENA->>Admin: POST /api/retrieveData<br/>Authorization: Bearer <token>
    Note right of Admin: Validates signature + claims
    Admin-->>DENA: 200 OK + data
```

### What the DENA team needs

To configure the connector with OAuth2, provide the DENA team with:

| Field | Description | Example |
|-------|-------------|---------|
| **Token URL** | Your IDP endpoint to obtain tokens | `https://your-idp.admin.eus/realms/your-realm/protocol/openid-connect/token` |
| **Client ID** | Identifier of the client you create for DENA in your IDP | `dena-core-client` |
| **Client Secret** | Secret associated with the client ID | (provided securely) |
| **Scopes** | Scopes required by your endpoint (if applicable) | `data-retrieve` |

---

### Token lifecycle

DENA automatically manages token acquisition and renewal:

``` mermaid
flowchart TD
    A[Incoming Data-Retrieve request] --> B{Valid token in cache?}
    B -->|Yes, not expired| D[Call admin endpoint]
    B -->|No or expired| C[Obtain new token from IDP]
    C --> D
    D --> E{401 response?}
    E -->|No| F[Return data]
    E -->|Yes| G[Invalidate token + retry]
    G --> C
```

| Behavior | Detail |
|----------|--------|
| **Cache** | DENA caches the token while it remains valid |
| **Leeway** | The token is renewed ~60s before expiration to avoid rejections due to latency |
| **Retry on 401** | If your endpoint returns 401, DENA invalidates the token and requests a new one |
| **Max retries** | 3 token acquisition attempts before failing |

---

### JWT token structure

The token DENA sends to your endpoint is a standard JWT with three parts: **header**, **payload**, and **signature**.

#### Header

```json
{
  "alg": "RS256",
  "typ": "JWT",
  "kid": "<key-id>"
}
```

#### Payload (claims)

```json
{
  "iss": "https://your-idp.admin.eus/realms/your-realm",
  "sub": "dena-core-client",
  "aud": "your-admin-resource",
  "exp": 1724500800,
  "iat": 1724500500,
  "jti": "550e8400-e29b-41d4-a716-446655440000",
  "azp": "dena-core-client",
  "scope": "data-retrieve",
  "client_id": "dena-core-client"
}
```

| Claim | Type | Description |
|-------|------|-------------|
| `iss` | `String` | Issuer URL (your IDP). Use it to validate that the token comes from your IDP |
| `sub` | `String` | Subject of the token. Normally the `client_id` of DENA in your IDP |
| `aud` | `String/Array` | Audience for which the token was issued. Your admin must verify it is the expected value |
| `exp` | `Number` | UNIX expiration timestamp. Reject expired tokens |
| `iat` | `Number` | UNIX issuance timestamp |
| `jti` | `String` | Unique token identifier (to prevent replay) |
| `azp` | `String` | Authorized party. The `client_id` of DENA registered in your IDP |
| `scope` | `String` | Granted scopes. Depends on your IDP configuration |

---

### Token verification in your endpoint

Your endpoint must validate the received token:

| Validation | What to check |
|------------|---------------|
| **Expiration** | `exp` > current time |
| **Issuer** | `iss` == your IDP URL |
| **Audience** | `aud` contains your resource |
| **Authorized client** | `azp` or `client_id` == the client_id DENA provided |

#### Example (Spring Security)

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .authorizeHttpRequests(auth -> auth
                .requestMatchers("/api/retrieveData").authenticated()
                .anyRequest().permitAll()
            )
            .oauth2ResourceServer(oauth2 -> oauth2
                .jwt(jwt -> jwt
                    .jwkSetUri("https://your-idp.admin.eus/realms/your-realm/protocol/openid-connect/certs")
                )
            );
        return http.build();
    }
}
```

```yaml
# application.yml
spring:
  security:
    oauth2:
      resourceserver:
        jwt:
          issuer-uri: https://your-idp.admin.eus/realms/your-realm
```

---

### IDP configuration

For DENA to be able to obtain tokens, create an OAuth2 client with this configuration:

| Parameter | Recommended value |
|-----------|-------------------|
| **Client ID** | `dena-core-client` (or the name you prefer) |
| **Client Authentication** | Enabled (confidential client) |
| **Grant type** | `client_credentials` |
| **Scopes** | Those required by your endpoint (e.g.: `data-retrieve`) |
| **Token lifetime** | 300 seconds (5 min) recommended |

!!! success "What you must provide to the DENA team"
    
    Once the client is created in your IDP:
    
    1. **Token URL** — E.g.: `https://your-idp.admin.eus/realms/your-realm/protocol/openid-connect/token`
    2. **Client ID** — The one you created for DENA
    3. **Client Secret** — The associated secret
    4. **Required scopes** — If your endpoint requires specific scopes (optional)

---

### Common errors (OAuth2)

| Error | Cause | Solution |
|-------|-------|----------|
| `invalid_client` | Incorrect client_id or client_secret | Verify credentials provided to DENA |
| `unauthorized_client` | Client does not have permission for `client_credentials` | Enable the grant type in your IDP |
| Connection timeout to IDP | Network not open | Open traffic from DENA IPs to your IDP |
| 401 on your endpoint | Token not recognized | Verify that your endpoint validates against the same IDP that issued the token |
| Expired token | High network latency | Verify that `expires_in` > 60s |

---

## Alternative mechanisms

If your administration **does not have an OAuth2 IDP**, that is not a problem. The connector can adapt to other security mechanisms. The DENA team develops the specific authentication logic for each case.

=== "mTLS (X.509 Certificates)"

    Mutual authentication at the TLS level. DENA presents a client certificate when connecting.
    
    **What you provide to DENA:**
    
    - Root certificate (CA) of your endpoint so DENA can trust it
    - A client certificate issued by your CA for DENA to use
    
    Your endpoint validates the certificate at the TLS layer.

=== "API Key"

    DENA includes a static key in a custom HTTP header.
    
    **What you provide to DENA:**
    
    - Header name (e.g.: `X-API-Key`)
    - Key value
    
    ```http
    POST /api/retrieveData
    X-API-Key: your-secret-api-key
    ```

=== "CAS"

    Central Authentication Service (institutional authentication).
    
    **What you provide to DENA:**
    
    - URL of your CAS service
    - Service account credentials
    - How to include the ticket in calls (header, parameter, etc.)

=== "Basic Auth"

    DENA includes username and password encoded in Base64.
    
    **What you provide to DENA:**
    
    - Username
    - Password
    
    ```http
    POST /api/retrieveData
    Authorization: Basic dXNlcjpwYXNz
    ```

=== "WS-Security / SOAP"

    For administrations with legacy SOAP services.
    
    **What you provide to DENA:**
    
    - Service WSDL
    - Credentials and/or certificates
    - Expected SOAP envelope format

=== "No authentication"

    Only for endpoints in internal networks (Euskalsarea) where security is guaranteed at the network level.
    
    **What you provide to DENA:**
    
    - Nothing (just confirm connectivity)

!!! tip "The connector is managed by DENA"
    Regardless of the mechanism you choose, **the connector is developed and maintained by the DENA team**. Your administration only needs to:
    
    1. Decide which security mechanism it offers
    2. Provide the necessary credentials / certificates / configuration
    3. Ensure your endpoint is accessible from the DENA network

---

## Summary of responsibilities

| Responsibility | Who |
|----------------|-----|
| Decide the security mechanism | :material-domain: Your administration |
| Provide credentials / config | :material-domain: Your administration |
| Develop the connector | :material-swap-horizontal: DENA team |
| Authenticate before each call | :material-swap-horizontal: DENA connector (automatic) |
| Validate authentication at the endpoint | :material-domain: Your administration |

---

## Contact

To coordinate the security configuration of your connector:

**:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

Please indicate:

- Which security mechanism your administration offers
- The credentials or configuration data needed
- Whether your endpoint is on the Internet or Euskalsarea

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
