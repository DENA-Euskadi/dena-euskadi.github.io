# :material-arrow-right-bold: Zure Sistemak DENA Deitzen Du

> **Bertsioa:** `v{{ dena.version }}` · **Data:** {{ dena.date }}

Zure administrazioak DENA-ri dei bat egiten dionean (Metadata-Sync bidali, Person-Sync Pull kontsultatu, etab.), autentifikazioa eta segurtasun-goiburuak sartu behar ditu.

---

## Autentifikazio-fluxua

``` mermaid
sequenceDiagram
    participant Admin as Zure Sistema
    participant IDP as Keycloak DENA
    participant DENA as CORE DENA

    Admin->>IDP: POST /token (client_credentials)
    IDP-->>Admin: access_token (JWT, 5 min)
    Admin->>DENA: POST /api/... + Bearer token + Security Headers
    DENA-->>Admin: 200 OK
```

---

## 1. OAuth2 tokena lortu

Zure sistemak JWT token bat lortzen du DENA-ren Identity Provider-etik `client_credentials` fluxua erabiliz.

### Endpoint-a

```
POST /realms/DenaAuthAdmins/protocol/openid-connect/token
Content-Type: application/x-www-form-urlencoded
```

### Parametroak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `grant_type` | `String` | :material-check: | `client_credentials` |
| `client_id` | `String` | :material-check: | DENA-k hornitu duen bezero-identifikadorea |
| `client_secret` | `String` | :material-check: | DENA-k hornitu duen bezero-sekretua |

### curl adibidea

```bash
curl -X POST \
  https://api-batera.pre.dena.eus/auth/realms/DenaAuthAdmins/protocol/openid-connect/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=<zure-client-id>&client_secret=<zure-client-secret>"
```

### Erantzun arrakastatsua (HTTP 200)

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

| Eremua | Deskribapena |
|---|---|
| `access_token` | `Authorization: Bearer <token>` goiburuan sartu beharreko JWT tokena |
| `expires_in` | Iraungitzera arte geratzen diren segundoak (normalean 300 = 5 min) |
| `token_type` | Beti `Bearer` |

### Errore-erantzuna (HTTP 4xx)

```json
{
    "error": "invalid_client",
    "error_description": "Invalid client or Invalid client credentials"
}
```

| Errorea | Kausa |
|---|---|
| `invalid_client` | `client_id` edo `client_secret` okerrak |
| `unauthorized_client` | Bezeroak ez du `client_credentials` erabiltzeko baimenik |
| `invalid_grant` | `grant_type` baliogabea |

### Tokenaren berritzea

!!! warning "Tokena 5 minututan iraungitzen da"
    - Gorde tokena cachean baliozkoa den bitartean
    - Berritu **leeway** batekin, iraungitu baino ~60 segundo lehenago
    - Ez eskatu token berri bat eskaera bakoitzeko

---

## 2. Segurtasun-goiburuak

Tokenaz gain, DENA-ri egindako dei bakoitzak segurtasun-goiburuak sartu behar ditu, mezuaren osotasuna eta trazabilitatea bermatzeko.

### Derrigorrezko goiburuak

| Header | Deskribapena | Adibidea |
|---|---|---|
| `Authorization` | Aurreko urratsean lortutako JWT tokena | `Bearer eyJhbGciOiJSUzI1NiIs...` |
| `Content-Type` | Eduki-mota | `application/json` |
| `X-DENA-Message-Correlation-ID` | Zure sistemak eskaera honetarako sortutako UUID bakarra. Fluxu osoan mantentzen da trazabilitaterako | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Zure sistemak eskaera abiarazten duen EPOCH unea (milisegundotan) | `1724500500000` |
| `X-DENA-Data-Digest` | Mezuaren gorputzaren SHA-256 hash-a (ikus behean nola sortu) | `sha-256=2251f5ac60e4...` |

### Aukerako goiburuak

| Header | Deskribapena | Adibidea |
|---|---|---|
| `Content-Digest` | Mezu osoaren SHA-256 hash-a (goiburuak + gorputza) | `sha-256=...` |
| `X-DENA-Origin-TimeStamp` | Jatorrizko osagaian fluxua hasi zen EPOCH unea. Osagaien artean aldatu gabe mantentzen da | `1724500500000` |
| `User-Agent` | Zure sistemaren identifikazioa. Gomendatutako formatua: `AdminX/<bert> <modulua>/<bert> (laguntza@admin.eus)` | `NireAdmin/1.0 srmd-sender/1.0 (laguntza@nireadmin.eus)` |

### Segurtasun-eremuen sorrera

#### X-DENA-Message-Correlation-ID

Sortu UUID v4 ausazko bat eskaera bakoitzeko. ID hau DENA-n zehar prozesatze osoan mantentzen da eta logetan agertzen da arazketa errazteko.

```java
String correlationId = UUID.randomUUID().toString();
// Emaitza: "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
```

#### X-DENA-This-TimeStamp

Uneko unea milisegundotan EPOCH-etik (1970-01-01T00:00:00Z):

```java
long timestamp = System.currentTimeMillis();
// Emaitza: 1724500500000
```

#### X-DENA-Data-Digest

Eskaeraren gorputzaren SHA-256 hash-a, hamaseitar formatuan kodetuta:

```java
import java.security.MessageDigest;
import java.nio.charset.StandardCharsets;

public static String computeDataDigest(String body) {
    MessageDigest digest = MessageDigest.getInstance("SHA-256");
    byte[] hash = digest.digest(body.getBytes(StandardCharsets.UTF_8));
    
    // Hamaseitar formatura bihurtu
    StringBuilder hex = new StringBuilder();
    for (byte b : hash) {
        hex.append(String.format("%02x", b));
    }
    return "sha-256=" + hex.toString();
}
```

```python
import hashlib

def compute_data_digest(body: str) -> str:
    hash_value = hashlib.sha256(body.encode('utf-8')).hexdigest()
    return f"sha-256={hash_value}"
```

!!! warning "Kalkulatu digest-a gorputz zehatzaren gainean"
    Hash-a sarean bidaltzen den gorputz zehatzaren gainean kalkulatu behar da (JSON serializazioaren ondoren). Proxy edo middleware batek gorputza kalkuluaren ondoren aldatzen badu, DENA-k eskaera baztertuko du.

---

## 3. Dei osoaren adibidea

Eskaera osoa DENA-ri goiburu guztiekin:

```bash
# 1. Segurtasun-balioak sortu
CORRELATION_ID=$(uuidgen | tr '[:upper:]' '[:lower:]')
TIMESTAMP=$(date +%s%3N)
BODY='[{"admin":{"id":"MI-ADMIN"},"aboutPerson":{"id":"12345678A"},"someDataWasUpdatedAt":"2026-08-24T10:30:00.000Z","ofType":{"id":"RECORDS"},"fromDataOrigin":"DEFAULT"}]'
DATA_DIGEST="sha-256=$(echo -n "$BODY" | shasum -a 256 | cut -d' ' -f1)"

# 2. Goiburu guztiekin bidali
curl -X POST "https://api-batera.pre.dena.eus/srmd/" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -H "X-DENA-Message-Correlation-ID: ${CORRELATION_ID}" \
  -H "X-DENA-This-TimeStamp: ${TIMESTAMP}" \
  -H "X-DENA-Data-Digest: ${DATA_DIGEST}" \
  -H "User-Agent: NireAdmin/1.0 srmd-sender/1.0 (laguntza@nireadmin.eus)" \
  -d "${BODY}"
```

---

## 4. Kredentzialak

!!! info "Nola lortu zure kredentzialak"

    Kredentzialak (`client_id` eta `client_secret`) DENA taldeak ematen ditu administrazioaren alta-fasean.
    
    Administrazio eta ingurune (PRE/PRO) bakoitzerako espezifikoak dira.
    
    **:material-email: Kontaktua:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

---

!!! tip "Postman"

    Postman bilduma eta environment-a eskuragarri daude [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman) helbidean.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
