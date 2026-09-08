# :material-arrow-left-bold: DENA Llama a Tu Sistema

Cuando DENA necesita llamar a tu administracion (para Data-Retrieve, por ejemplo), debe autenticarse contra tu sistema. El **conector** de tu administracion es el componente que gestiona esta autenticacion.

---

## Principio fundamental

!!! info "El conector se adapta a ti"
    Cada administracion puede ofrecer el mecanismo de seguridad que prefiera. El **conector** — desarrollado y mantenido por el equipo DENA — es el que se encarga de autenticarse contra tu sistema usando el mecanismo que tu administracion exponga.
    
    Tu administracion **no necesita implementar nada especial para DENA**. Solo necesita indicar al equipo DENA como autenticarse.

---

## Flujo estandar: OAuth2 client_credentials

El mecanismo recomendado y mas habitual es **OAuth2 `client_credentials`**. Si tu administracion dispone de un Identity Provider (Keycloak, ADFS, Cognito, Azure AD...), este es el flujo:

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (Conector)
    participant IDP as IDP de tu administracion
    participant Admin as Tu endpoint REST

    DENA->>IDP: POST /token (client_credentials)
    Note right of IDP: Valida client_id + client_secret
    IDP-->>DENA: access_token (JWT)
    DENA->>Admin: POST /api/retrieveData<br/>Authorization: Bearer <token>
    Note right of Admin: Valida firma + claims
    Admin-->>DENA: 200 OK + datos
```

### Que necesita el equipo DENA

Para configurar el conector con OAuth2, proporciona al equipo DENA:

| Dato | Descripcion | Ejemplo |
|------|-------------|---------|
| **Token URL** | Endpoint de tu IDP para obtener tokens | `https://tu-idp.admin.eus/realms/tu-realm/protocol/openid-connect/token` |
| **Client ID** | Identificador del cliente que creas para DENA en tu IDP | `dena-core-client` |
| **Client Secret** | Secreto asociado al client ID | (proporcionado de forma segura) |
| **Scopes** | Scopes requeridos por tu endpoint (si aplica) | `data-retrieve` |

---

### Ciclo de vida del token

DENA gestiona automaticamente la obtencion y renovacion de tokens:

``` mermaid
flowchart TD
    A[Peticion Data-Retrieve entrante] --> B{Token en cache valido?}
    B -->|Si, no expirado| D[Llamar endpoint admin]
    B -->|No o expirado| C[Obtener nuevo token del IDP]
    C --> D
    D --> E{Respuesta 401?}
    E -->|No| F[Devolver datos]
    E -->|Si| G[Invalidar token + reintentar]
    G --> C
```

| Comportamiento | Detalle |
|----------------|---------|
| **Cache** | DENA cachea el token mientras sea valido |
| **Leeway** | El token se renueva ~60s antes de expirar para evitar rechazos por latencia |
| **Retry en 401** | Si tu endpoint devuelve 401, DENA invalida el token y solicita uno nuevo |
| **Max reintentos** | 3 intentos de obtencion de token antes de fallar |

---

### Estructura del token JWT

El token que DENA envia a tu endpoint es un JWT estandar con tres partes: **header**, **payload** y **signature**.

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
  "iss": "https://tu-idp.admin.eus/realms/tu-realm",
  "sub": "dena-core-client",
  "aud": "tu-admin-resource",
  "exp": 1724500800,
  "iat": 1724500500,
  "jti": "550e8400-e29b-41d4-a716-446655440000",
  "azp": "dena-core-client",
  "scope": "data-retrieve",
  "client_id": "dena-core-client"
}
```

| Claim | Tipo | Descripcion |
|-------|------|-------------|
| `iss` | `String` | URL del emisor (tu IDP). Usalo para validar que el token proviene de tu IDP |
| `sub` | `String` | Sujeto del token. Normalmente el `client_id` de DENA en tu IDP |
| `aud` | `String/Array` | Audiencia para la que se emitio el token. Tu admin debe verificar que es el valor esperado |
| `exp` | `Number` | Timestamp UNIX de expiracion. Rechazar tokens expirados |
| `iat` | `Number` | Timestamp UNIX de emision |
| `jti` | `String` | Identificador unico del token (para prevenir replay) |
| `azp` | `String` | Authorized party. El `client_id` de DENA registrado en tu IDP |
| `scope` | `String` | Scopes concedidos. Depende de la configuracion de tu IDP |

---

### Verificacion del token en tu endpoint

Tu endpoint debe validar el token recibido:

| Validacion | Que comprobar |
|------------|---------------|
| **Expiracion** | `exp` > tiempo actual |
| **Emisor** | `iss` == URL de tu IDP |
| **Audiencia** | `aud` contiene tu recurso |
| **Cliente autorizado** | `azp` o `client_id` == el client_id que DENA proporciono |

#### Ejemplo (Spring Security)

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
                    .jwkSetUri("https://tu-idp.admin.eus/realms/tu-realm/protocol/openid-connect/certs")
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
          issuer-uri: https://tu-idp.admin.eus/realms/tu-realm
```

---

### Configuracion de tu IDP

Para que DENA pueda obtener tokens, crea un cliente OAuth2 con esta configuracion:

| Parametro | Valor recomendado |
|-----------|-------------------|
| **Client ID** | `dena-core-client` (o el nombre que prefieras) |
| **Client Authentication** | Activado (confidential client) |
| **Grant type** | `client_credentials` |
| **Scopes** | Los que necesite tu endpoint (ej: `data-retrieve`) |
| **Token lifetime** | 300 segundos (5 min) recomendado |

!!! success "Lo que debes proporcionar al equipo DENA"
    
    Una vez creado el cliente en tu IDP:
    
    1. **Token URL** — Ej: `https://tu-idp.admin.eus/realms/tu-realm/protocol/openid-connect/token`
    2. **Client ID** — El que hayas creado para DENA
    3. **Client Secret** — El secreto asociado
    4. **Scopes requeridos** — Si tu endpoint requiere scopes especificos (opcional)

---

### Errores comunes (OAuth2)

| Error | Causa | Solucion |
|-------|-------|----------|
| `invalid_client` | client_id o client_secret incorrectos | Verificar credenciales proporcionadas a DENA |
| `unauthorized_client` | El cliente no tiene permiso para `client_credentials` | Habilitar el grant type en tu IDP |
| Connection timeout al IDP | Red no abierta | Abrir trafico desde IPs de DENA hacia tu IDP |
| 401 en tu endpoint | Token no reconocido | Verificar que tu endpoint valida contra el mismo IDP que emitio el token |
| Token expirado | Latencia de red alta | Verificar que `expires_in` > 60s |

---

## Mecanismos alternativos

Si tu administracion **no dispone de un IDP OAuth2**, no hay problema. El conector puede adaptarse a otros mecanismos de seguridad. El equipo DENA desarrolla la logica de autenticacion especifica para cada caso.

=== "mTLS (Certificados X.509)"

    Autenticacion mutua a nivel TLS. DENA presenta un certificado cliente al conectar.
    
    **Que proporcionas a DENA:**
    
    - Certificado raiz (CA) de tu endpoint para que DENA confie en el
    - Un certificado cliente emitido por tu CA para que DENA lo use
    
    Tu endpoint valida el certificado en la capa TLS.

=== "API Key"

    DENA incluye una clave estatica en una cabecera HTTP personalizada.
    
    **Que proporcionas a DENA:**
    
    - Nombre de la cabecera (ej: `X-API-Key`)
    - Valor de la clave
    
    ```http
    POST /api/retrieveData
    X-API-Key: tu-api-key-secreta
    ```

=== "CAS"

    Central Authentication Service (autenticacion institucional).
    
    **Que proporcionas a DENA:**
    
    - URL de tu servicio CAS
    - Credenciales de servicio (service account)
    - Como incluir el ticket en las llamadas (header, parametro, etc.)

=== "Basic Auth"

    DENA incluye usuario y contrasena codificados en Base64.
    
    **Que proporcionas a DENA:**
    
    - Username
    - Password
    
    ```http
    POST /api/retrieveData
    Authorization: Basic dXNlcjpwYXNz
    ```

=== "WS-Security / SOAP"

    Para administraciones con servicios SOAP legacy.
    
    **Que proporcionas a DENA:**
    
    - WSDL del servicio
    - Credenciales y/o certificados
    - Formato del sobre SOAP esperado

=== "Sin autenticacion"

    Solo para endpoints en redes internas (Euskalsarea) donde la seguridad se garantiza a nivel de red.
    
    **Que proporcionas a DENA:**
    
    - Nada (solo confirmar conectividad)

!!! tip "El conector lo gestiona DENA"
    Independientemente del mecanismo que elijas, **el conector lo desarrolla y mantiene el equipo DENA**. Tu administracion solo necesita:
    
    1. Decidir que mecanismo de seguridad ofrece
    2. Proporcionar las credenciales / certificados / configuracion necesaria
    3. Asegurar que tu endpoint es accesible desde la red de DENA

---

## Resumen de responsabilidades

| Responsabilidad | Quien |
|----------------|-------|
| Decidir el mecanismo de seguridad | :material-domain: Tu administracion |
| Proporcionar credenciales / config | :material-domain: Tu administracion |
| Desarrollar el conector | :material-swap-horizontal: Equipo DENA |
| Autenticarse antes de cada llamada | :material-swap-horizontal: Conector DENA (automatico) |
| Validar la autenticacion en el endpoint | :material-domain: Tu administracion |

---

## Contacto

Para coordinar la configuracion de seguridad de tu conector:

**:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

Indica:

- Que mecanismo de seguridad ofrece tu administracion
- Las credenciales o datos de configuracion necesarios
- Si tu endpoint esta en Internet o Euskalsarea

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
