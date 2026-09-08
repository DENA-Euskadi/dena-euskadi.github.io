# :material-arrow-left-bold: DENAk Zure Sistemari Deitzen Dio

DENAk zure administrazioari deitu behar dionean (Data-Retrieve egiteko, adibidez), zure sistemaren aurka autentifikatu behar da. Zure administrazioaren **konektorea** da autentifikazio hori kudeatzen duen osagaia.

---

## Oinarrizko printzipioa

!!! info "Konektorea zuri egokitzen zaizu"
    Administrazio bakoitzak nahi duen segurtasun-mekanismoa eskain dezake. **Konektorea** — DENA taldeak garatua eta mantendua — zure sistemaren aurka autentifikatzeaz arduratzen da, zure administrazioak eskaintzen duen mekanismoa erabiliz.
    
    Zure administrazioak **ez du ezer berezirik inplementatu behar DENArentzat**. DENA taldeari nola autentifikatu behar den adierazi besterik ez du egin behar.

---

## Fluxu estandarra: OAuth2 client_credentials

Gomendatutako eta ohikoena den mekanismoa **OAuth2 `client_credentials`** da. Zure administrazioak Identity Provider bat badu (Keycloak, ADFS, Cognito, Azure AD...), hau da fluxua:

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (Konektorea)
    participant IDP as Zure administrazioaren IDP
    participant Admin as Zure REST endpoint-a

    DENA->>IDP: POST /token (client_credentials)
    Note right of IDP: client_id + client_secret balioztatu
    IDP-->>DENA: access_token (JWT)
    DENA->>Admin: POST /api/retrieveData<br/>Authorization: Bearer <token>
    Note right of Admin: Sinadura + claims balioztatu
    Admin-->>DENA: 200 OK + datuak
```

### Zer behar du DENA taldeak

Konektorea OAuth2rekin konfiguratzeko, eman DENA taldeari honako hauek:

| Datua | Deskribapena | Adibidea |
|-------|--------------|----------|
| **Token URL** | Zure IDParen endpoint-a tokenak lortzeko | `https://zure-idp.admin.eus/realms/zure-realm/protocol/openid-connect/token` |
| **Client ID** | Zure IDPan DENArentzat sortzen duzun bezeroaren identifikatzailea | `dena-core-client` |
| **Client Secret** | Client IDari lotutako sekretua | (modu seguruan emandakoa) |
| **Scopes** | Zure endpoint-ak eskatzen dituen scope-ak (aplikagarria bada) | `data-retrieve` |

---

### Tokenaren bizi-zikloa

DENAk automatikoki kudeatzen du tokenen lorpena eta berrikuntza:

``` mermaid
flowchart TD
    A[Data-Retrieve eskaera sarrera] --> B{Token baliozkoa cachean?}
    B -->|Bai, iraungitu gabe| D[Admin endpoint-ari deitu]
    B -->|Ez edo iraungita| C[Token berria lortu IDPtik]
    C --> D
    D --> E{401 erantzuna?}
    E -->|Ez| F[Datuak itzuli]
    E -->|Bai| G[Tokena baliogabetu + berriro saiatu]
    G --> C
```

| Portaera | Xehetasuna |
|----------|------------|
| **Cache** | DENAk tokena cachean gordetzen du baliozkoa den bitartean |
| **Leeway** | Tokena ~60s lehenago berritzen da iraungitu baino, latentzia-arrazoiengatik bazterketak ekiditeko |
| **401 saiakera** | Zure endpoint-ak 401 itzultzen badu, DENAk tokena baliogabetzen du eta berri bat eskatzen du |
| **Gehienezko saiakerak** | 3 saiakera tokena lortzeko huts egin aurretik |

---

### JWT tokenaren egitura

DENAk zure endpoint-era bidaltzen duen tokena JWT estandar bat da hiru zatirekin: **header**, **payload** eta **signature**.

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
  "iss": "https://zure-idp.admin.eus/realms/zure-realm",
  "sub": "dena-core-client",
  "aud": "zure-admin-resource",
  "exp": 1724500800,
  "iat": 1724500500,
  "jti": "550e8400-e29b-41d4-a716-446655440000",
  "azp": "dena-core-client",
  "scope": "data-retrieve",
  "client_id": "dena-core-client"
}
```

| Claim | Mota | Deskribapena |
|-------|------|--------------|
| `iss` | `String` | Jaulkitzailearen URLa (zure IDPa). Erabili tokena zure IDPtik datorrela balidatzeko |
| `sub` | `String` | Tokenaren subjektua. Normalean DENAren `client_id` zure IDPan |
| `aud` | `String/Array` | Tokena zeinentzat jaulki zen. Zure admin-ak espero den balioa dela egiaztatu behar du |
| `exp` | `Number` | Iraungitze UNIX timestamp-a. Iraungitako tokenak baztertu |
| `iat` | `Number` | Jaulkitze UNIX timestamp-a |
| `jti` | `String` | Tokenaren identifikatzaile bakarra (replay ekiditeko) |
| `azp` | `String` | Authorized party. Zure IDPan erregistratutako DENAren `client_id` |
| `scope` | `String` | Emandako scope-ak. Zure IDParen konfigurazioaren araberakoa |

---

### Tokena zure endpoint-ean egiaztatzea

Zure endpoint-ak jasotako tokena balidatu behar du:

| Balidazioa | Zer egiaztatu |
|------------|---------------|
| **Iraungitzea** | `exp` > uneko denbora |
| **Jaulkitzailea** | `iss` == zure IDParen URLa |
| **Audientzia** | `aud`-k zure baliabidea dauka |
| **Baimendutako bezeroa** | `azp` edo `client_id` == DENAk eman duen client_id |

#### Adibidea (Spring Security)

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
                    .jwkSetUri("https://zure-idp.admin.eus/realms/zure-realm/protocol/openid-connect/certs")
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
          issuer-uri: https://zure-idp.admin.eus/realms/zure-realm
```

---

### Zure IDParen konfigurazioa

DENAk tokenak lor ditzan, sortu OAuth2 bezero bat konfigurazio honekin:

| Parametroa | Gomendatutako balioa |
|------------|----------------------|
| **Client ID** | `dena-core-client` (edo nahiago duzun izena) |
| **Client Authentication** | Aktibatuta (confidential client) |
| **Grant type** | `client_credentials` |
| **Scopes** | Zure endpoint-ak behar dituenak (adib.: `data-retrieve`) |
| **Token lifetime** | 300 segundo (5 min) gomendatua |

!!! success "DENA taldeari eman behar diozuna"
    
    Bezeroa zure IDPan sortutakoan:
    
    1. **Token URL** — Adib.: `https://zure-idp.admin.eus/realms/zure-realm/protocol/openid-connect/token`
    2. **Client ID** — DENArentzat sortutakoa
    3. **Client Secret** — Lotutako sekretua
    4. **Beharrezko scope-ak** — Zure endpoint-ak scope zehatzak eskatzen baditu (aukerakoa)

---

### Errore arruntak (OAuth2)

| Errorea | Kausa | Konponbidea |
|---------|-------|-------------|
| `invalid_client` | client_id edo client_secret okerrak | DENAri emandako kredentzialak egiaztatu |
| `unauthorized_client` | Bezeroak ez du baimenik `client_credentials`-erako | Grant type-a gaitu zure IDPan |
| Connection timeout IDPra | Sarea itxita | Trafikoa ireki DENAren IPetatik zure IDPra |
| 401 zure endpoint-ean | Tokena ez da ezagutzen | Zure endpoint-ak tokena jaulki duen IDP beraren aurka balioztatzen duela egiaztatu |
| Token iraungitua | Sare-latentzia altua | `expires_in` > 60s dela egiaztatu |

---

## Mekanismo alternatiboak

Zure administrazioak **OAuth2 IDPrik ez badu**, ez da arazorik. Konektorea beste segurtasun-mekanismo batzuetara egokitu daiteke. DENA taldeak kasu bakoitzerako autentifikazio-logika espezifikoa garatzen du.

=== "mTLS (X.509 Ziurtagiriak)"

    Elkarrekiko autentifikazioa TLS mailan. DENAk bezero-ziurtagiri bat aurkezten du konektatzean.
    
    **Zer ematen diozu DENAri:**
    
    - Zure endpoint-aren erro-ziurtagiria (CA), DENAk beragan fidatu dezan
    - Zure CAk jaulkitako bezero-ziurtagiri bat, DENAk erabil dezan
    
    Zure endpoint-ak ziurtagiria TLS geruzan balioztatzen du.

=== "API Key"

    DENAk gako estatiko bat sartzen du HTTP goiburu pertsonalizatu batean.
    
    **Zer ematen diozu DENAri:**
    
    - Goiburuaren izena (adib.: `X-API-Key`)
    - Gakoaren balioa
    
    ```http
    POST /api/retrieveData
    X-API-Key: zure-api-key-sekretua
    ```

=== "CAS"

    Central Authentication Service (autentifikazio instituzionala).
    
    **Zer ematen diozu DENAri:**
    
    - Zure CAS zerbitzuaren URLa
    - Zerbitzu-kontuaren kredentzialak (service account)
    - Nola txertatu ticket-a deietan (goiburu, parametro, etab.)

=== "Basic Auth"

    DENAk erabiltzaile-izena eta pasahitza Base64-n kodetuta sartzen ditu.
    
    **Zer ematen diozu DENAri:**
    
    - Erabiltzaile-izena
    - Pasahitza
    
    ```http
    POST /api/retrieveData
    Authorization: Basic dXNlcjpwYXNz
    ```

=== "WS-Security / SOAP"

    SOAP zerbitzu zaharrak dituzten administrazioentzat.
    
    **Zer ematen diozu DENAri:**
    
    - Zerbitzuaren WSDLa
    - Kredentzialak eta/edo ziurtagiriak
    - Espero den SOAP gutun-azalaren formatua

=== "Autentifikaziorik gabe"

    Bakarrik barne-sareetako endpoint-entzat (Euskalsarea) non segurtasuna sare-mailan bermatzen den.
    
    **Zer ematen diozu DENAri:**
    
    - Ezer ez (konektibitatea baieztatu besterik ez)

!!! tip "Konektorea DENAk kudeatzen du"
    Aukeratzen duzun mekanismoa edozein dela ere, **konektorea DENA taldeak garatzen eta mantentzen du**. Zure administrazioak honako hau besterik ez du egin behar:
    
    1. Zein segurtasun-mekanismo eskaintzen duen erabaki
    2. Beharrezko kredentzialak / ziurtagiriak / konfigurazioa eman
    3. Zure endpoint-a DENAren saretik eskuragarri dagoela bermatu

---

## Erantzukizunen laburpena

| Erantzukizuna | Nork |
|---------------|------|
| Segurtasun-mekanismoa erabaki | :material-domain: Zure administrazioa |
| Kredentzialak / konfigurazioa eman | :material-domain: Zure administrazioa |
| Konektorea garatu | :material-swap-horizontal: DENA taldea |
| Dei bakoitzaren aurretik autentifikatu | :material-swap-horizontal: DENA konektorea (automatikoa) |
| Endpoint-ean autentifikazioa balidatu | :material-domain: Zure administrazioa |

---

## Kontaktua

Zure konektorearen segurtasun-konfigurazioa koordinatzeko:

**:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

Adierazi:

- Zein segurtasun-mekanismo eskaintzen duen zure administrazioak
- Beharrezko kredentzialak edo konfigurazio-datuak
- Zure endpoint-a Interneten edo Euskalsarean dagoen

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
