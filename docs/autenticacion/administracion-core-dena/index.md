# :material-arrow-right-bold: Tu Sistema Llama a DENA

> **Version:** `v{{ dena.version }}` · **Fecha:** {{ dena.date }}

Cuando tu administracion realiza una llamada a DENA (enviar Metadata-Sync, consultar Person-Sync Pull, etc.), debe incluir autenticacion y cabeceras de seguridad.

---

## Flujo de autenticacion

``` mermaid
sequenceDiagram
    participant Admin as Tu Sistema
    participant IDP as Keycloak DENA
    participant DENA as CORE DENA

    Admin->>IDP: POST /token (client_credentials)
    IDP-->>Admin: access_token (JWT, 5 min)
    Admin->>DENA: POST /api/... + Bearer token + Security Headers
    DENA-->>Admin: 200 OK
```

---

## 1. Obtener el token OAuth2

Tu sistema obtiene un token JWT del Identity Provider de DENA usando el flujo `client_credentials`.

### Endpoint

```
POST /realms/DenaAuthAdmins/protocol/openid-connect/token
Content-Type: application/x-www-form-urlencoded
```

### Parametros

| Campo | Tipo | Obligatorio | Descripcion |
|---|---|:---:|---|
| `grant_type` | `String` | :material-check: | `client_credentials` |
| `client_id` | `String` | :material-check: | Id de cliente provisionado por DENA |
| `client_secret` | `String` | :material-check: | Secreto de cliente provisionado por DENA |

### Ejemplo curl

```bash
curl -X POST \
  https://api-batera.pre.dena.eus/auth/realms/DenaAuthAdmins/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=<tu-client-id>&client_secret=<tu-client-secret>"
```

### Respuesta exitosa (HTTP 200)

```json
{
    "access_token": "eyJhbGciOiJSUzI1NiIs...",
    "expires_in": 300,
    "refresh_expires_in": 0,
    "token_type": "Bearer",
    "not-before-policy": 0,
    "scope": "email profile"
}
```

| Campo | Descripcion |
|---|---|
| `access_token` | Token JWT a incluir en la cabecera `Authorization: Bearer <token>` |
| `expires_in` | Segundos hasta expiracion (tipicamente 300 = 5 min) |
| `token_type` | Siempre `Bearer` |

### Respuesta de error (HTTP 4xx)

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
| `invalid_grant` | `grant_type` no valido |

### Renovacion del token

!!! warning "El token expira en 5 minutos"
    - Cachea el token mientras sea valido
    - Renovalo con un **leeway** de ~60 segundos antes de la expiracion
    - No solicites un token nuevo en cada peticion

---

## 2. Cabeceras de seguridad

Ademas del token, cada llamada a DENA debe incluir cabeceras de seguridad que garantizan la integridad y trazabilidad del mensaje.

### Cabeceras obligatorias

| Header | Descripcion | Ejemplo |
|---|---|---|
| `Authorization` | Token JWT obtenido en el paso anterior | `Bearer eyJhbGciOiJSUzI1NiIs...` |
| `Content-Type` | Tipo de contenido | `application/json` |
| `X-DENA-Message-Correlation-Id` | UUID unico generado por tu sistema para esta peticion. Se mantiene en todo el flujo para trazabilidad | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Instante EPOCH (milisegundos) en el que tu sistema inicia la peticion | `1724500500000` |
| `X-DENA-Data-Digest` | Hash SHA-256 de la concatenación `timestamp + correlationId + body`, codificado en Base64 (ver generación más abajo) | `SHA-256=:Rp/u+lTGp+VLFBrJGpC3BPw5Gf...=:` |

### Cabeceras opcionales

| Header | Descripcion | Ejemplo |
|---|---|---|
| `Content-Digest` | Hash SHA-256 del body, codificado en Base64. Complementa `X-DENA-Data-Digest` | `SHA-256=:Rp/u+lTGp+VLFBrJGpC3BPw5Gf...=:` |
| `X-DENA-Origin-TimeStamp` | Instante EPOCH del inicio del flujo en el componente original. Se conserva inalterado entre componentes | `1724500500000` |
| `User-Agent` | Identificacion de tu sistema. Formato recomendado: `AdminX/<ver> <modulo>/<ver> (soporte@admin.eus)` | `MiAdmin/1.0 srmd-sender/1.0 (soporte@miadmin.eus)` |

### Generacion de los campos de seguridad

#### X-DENA-Message-Correlation-Id

Genera un UUID v4 aleatorio por cada peticion. Este ID se mantiene durante todo el procesamiento en DENA y aparece en los logs para facilitar la depuracion.

```java
String correlationId = UUID.randomUUID().toString();
// Resultado: "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
```

#### X-DENA-This-TimeStamp

Instante actual en milisegundos desde EPOCH (1970-01-01T00:00:00Z):

```java
long timestamp = System.currentTimeMillis();
// Resultado: 1724500500000
```

#### X-DENA-Data-Digest

Hash SHA-256 de la concatenación **`timestamp + correlationId + body`**, codificado en **Base64**. El formato del valor es `SHA-256=:{base64}:`.

```java
import java.security.MessageDigest;
import java.nio.charset.StandardCharsets;
import java.util.Base64;

// X-DENA-Data-Digest = SHA-256=:base64(SHA256(timestamp + correlationId + body)):
public static String computeDenaDataDigest(String timestamp, String correlationId, String body)
        throws Exception {
    MessageDigest md = MessageDigest.getInstance("SHA-256");
    byte[] hash = md.digest((timestamp + correlationId + body).getBytes(StandardCharsets.UTF_8));
    return "SHA-256=:" + Base64.getEncoder().encodeToString(hash) + ":";
}
```

```python
import hashlib
import base64

# X-DENA-Data-Digest = SHA-256=:base64(SHA256(timestamp + correlationId + body)):
def compute_dena_data_digest(timestamp: str, correlation_id: str, body: str) -> str:
    data = (timestamp + correlation_id + body).encode('utf-8')
    hash_bytes = hashlib.sha256(data).digest()
    return "SHA-256=:" + base64.b64encode(hash_bytes).decode('utf-8') + ":"
```

!!! warning "El digest incluye timestamp y correlationId, no solo el body"
    La entrada del hash es la concatenación `timestamp + correlationId + body` (sin separadores). Calcular el hash solo sobre el body producirá un digest inválido y DENA rechazará la petición con 401.

---

## 3. Ejemplo completo de llamada

Peticion completa a DENA con todas las cabeceras:

```bash
# 1. Generar valores de seguridad
CORRELATION_ID=$(uuidgen | tr '[:upper:]' '[:lower:]')
TIMESTAMP=$(date +%s%3N)
BODY='[{"admin":{"id":"MI-ADMIN"},"aboutPerson":{"id":"12345678A"},"someDataWasUpdatedAt":"2026-08-24T10:30:00.0000000Z","ofType":{"id":"ADMINISTRATIVE_RECORD"},"fromDataOriginInstance":"DEFAULT"}]'
DATA_DIGEST="SHA-256=:$(printf '%s' "${TIMESTAMP}${CORRELATION_ID}${BODY}" | openssl dgst -sha256 -binary | openssl base64 -A):"

# 2. Enviar con todas las cabeceras
curl -X POST "https://api-batera.pre.dena.eus/srmd/" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "X-DENA-Message-Correlation-Id: ${CORRELATION_ID}" \
  -H "X-DENA-This-TimeStamp: ${TIMESTAMP}" \
  -H "X-DENA-Data-Digest: ${DATA_DIGEST}" \
  -H "User-Agent: MiAdmin/1.0 srmd-sender/1.0 (soporte@miadmin.eus)" \
  -d "${BODY}"
```

---

## 4. Credenciales

!!! info "Como obtener tus credenciales"

    Las credenciales (`client_id` y `client_secret`) las proporciona el equipo DENA durante la fase de alta de la administracion.
    
    Son especificas para cada administracion y entorno (PRE/PRO).
    
    **:material-email: Contacto:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

---

!!! tip "Postman"

    Coleccion y environment Postman disponibles en [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
