# :material-play-circle-outline: End-to-End adibidea

Kredentzialak lortzeko momentutik datuak jasotzera arteko fluxu osoa, urratsez urrats `curl` erabiliz.

!!! info "Aurrebaldintzak"
    - DENA taldeak emandako `client_id` eta `client_secret` edukitzea
    - DENA-ren PRE ingurunerantz HTTPS konektibitatea izatea
    - `curl` eta `jq` instalatuta egotea (edo edozein HTTP bezero)

---

## Fluxu osoa

``` mermaid
sequenceDiagram
    autonumber
    participant Admin as Zure Sistema
    participant IDP as Keycloak DENA
    participant DENA as DENA-CORE

    Admin->>IDP: 1. POST /token (client_credentials)
    IDP-->>Admin: 2. access_token (JWT, 5 min)
    Admin->>DENA: 3. POST /srmd + Bearer token
    DENA-->>Admin: 4. 200 OK (SRMD prozesatuak)
```

Adibide honek **Administrazioa → DENA** fluxua erakusten du (Metadata-Sync bidali). **DENA → Administrazioa** fluxua (Data-Retrieve) DENA-k automatikoki abiarazten du — zuk endpointa azaldu besterik ez duzu.

---

## 1. urratsa: OAuth2 token bat lortu

```bash
# Konfigurazio-aldagaiak
KEYCLOAK_URL="https://api-batera.pre.dena.eus/auth"
REALM="DenaAuthAdmins"
CLIENT_ID="zure-client-id"
CLIENT_SECRET="zure-client-secret"

# Tokena lortu
TOKEN=$(curl -s -X POST \
  "${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}" \
  | jq -r '.access_token')

echo "Tokena lortua: ${TOKEN:0:20}..."
```

### Espero den erantzuna

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "expires_in": 300,
  "token_type": "Bearer",
  "scope": "email profile"
}
```

!!! warning "Tokena 5 minututan iraungitzen da"
    Berritu tokena iraungitu baino lehen. Gomendagarria da ~60 segundo falta direnean berritzea (leeway).

---

## 2. urratsa: Metadata-Sync bidali (SRMD)

Lortutako tokenarekin, jakinarazi DENA-ri pertsona baten datu berriak daudela:

```bash
DENA_URL="https://api-batera.pre.dena.eus"

curl -s -X POST "${DENA_URL}/srmd/" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '[
    {
      "admin": { "id": "ZURE-ADMIN-ID" },
      "aboutPerson": { "id": "12345678A" },
      "someDataWasUpdatedAt": "2026-08-24T10:30:00.000Z",
      "ofType": { "id": "RECORDS" },
      "fromDataOrigin": "DEFAULT"
    }
  ]' | jq .
```

### Espero den erantzuna (200 OK)

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 1,
  "processedOK": [
    {
      "admin": { "id": "ZURE-ADMIN-ID" },
      "aboutPerson": { "id": "12345678A" },
      "ofType": { "id": "RECORDS" }
    }
  ],
  "processedNOK": []
}
```

!!! success "Eginda"
    DENA-k orain badaki `12345678A` pertsonaren espedienteen datu eguneratuak dituzula. Pertsonak aplikazioa irekitzen duenean, DENA-k zure Data-Retrieve endpointera deituko du datu horiek lortzeko.

---

## 3. urratsa: Zure Data-Retrieve endpointa azaldu

DENA-k zure sistemara deituko du pertsonak bere datuak ikusi behar dituenean. Zure endpointak:

1. `POST` bat jaso behar du pertsonaren id-arekin (`subjectPerson.id`) eta datu-motarekin (`dataType.id`)
2. Zure datu-basea kontsultatu
3. Datuak DENA formatuan itzuli

### DENA-tik jasoko duzun eskaera (Request)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": []
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "administrativeServiceProcedureRecord" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-0001" }
  },
  "payload": {}
}
```

### Itzuli behar duzun erantzuna (Response)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": []
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "administrativeServiceProcedureRecord" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-0001" }
  },
  "payload": {
    "dataItems": [
      {
        "oid": "EXP-2026-001",
        "id": "2026/00456",
        "lastChangedAt": "2026-08-24T10:30:00.000Z",
        "service": {
          "serviceNameByLanguage": {
            "SPANISH": "Licencias de actividad",
            "BASQUE": "Jarduera-lizentziak"
          },
          "originRef": { "id": "SRV-LIC-001" }
        },
        "procedure": {
          "serviceNameByLanguage": {
            "SPANISH": "Solicitud de licencia de apertura",
            "BASQUE": "Irekiera-lizentzia eskaera"
          },
          "originRef": { "id": "PROC-LIC-001" }
        },
        "createdAt": "2026-06-15T09:00:00.000Z",
        "state": {
          "stateCode": "IN_PROGRESS",
          "description": {
            "SPANISH": "En tramitacion",
            "BASQUE": "Izapidetzen"
          }
        },
        "urls": [
          { "url": "https://sede.tuadmin.eus/expedientes/2026-00456", "language": "SPANISH", "tags": ["default"] },
          { "url": "https://egoitza.tuadmin.eus/espedienteak/2026-00456", "language": "BASQUE", "tags": ["default"] }
        ]
      }
    ]
  },
  "code": "OK"
}
```

---

## 4. urratsa: curl-ekin egiaztatu (DENA simulatuz)

Benetako integrazioa egin aurretik zure endpointa lokalean probatzeko:

```bash
curl -s -X POST http://localhost:8080/api/retrieveData \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "context": {
      "message": {
        "type": "PERSON_FETCH_DATA",
        "correlationId": "test-001",
        "interopRouteData": []
      },
      "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "administrativeServiceProcedureRecord" },
      "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-0001" }
    },
    "payload": {}
  }' | jq .
```

### Balidazio-zerrenda

- [ ] HTTP 200 itzultzen du `code: "OK"` batekin
- [ ] `dataItems` array bat da (hutsik daturik ez badago)
- [ ] Elementu bakoitzak `oid`, `id`, `lastChangedAt` ditu
- [ ] Testuek `SPANISH` eta `BASQUE` barne hartzen dute
- [ ] Eskaeran dagoen `context.message.correlationId` erantzunean mantentzen da
- [ ] Egoitza elektronikoaren URLak hizkuntza bakoitzeko sartuta daude
- [ ] Erantzun-denbora < 30 segundo

---

## Script osoa

Bash script batek end-to-end fluxu osoa exekutatzen du:

```bash
#!/bin/bash
# dena-e2e-test.sh — End-to-end proba DENA PRE-ren aurka
set -e

# === KONFIGURAZIOA ===
KEYCLOAK_URL="https://api-batera.pre.dena.eus/auth"
REALM="DenaAuthAdmins"
CLIENT_ID="${DENA_CLIENT_ID:?Konfiguratu DENA_CLIENT_ID}"
CLIENT_SECRET="${DENA_CLIENT_SECRET:?Konfiguratu DENA_CLIENT_SECRET}"
DENA_URL="https://api-batera.pre.dena.eus"
ADMIN_ID="ZURE-ADMIN-ID"

echo "=== 1. Tokena lortzen ==="
RESPONSE=$(curl -s -X POST \
  "${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}")

TOKEN=$(echo "$RESPONSE" | jq -r '.access_token')
EXPIRES=$(echo "$RESPONSE" | jq -r '.expires_in')

if [ "$TOKEN" = "null" ] || [ -z "$TOKEN" ]; then
  echo "ERROREA: Ezin izan da tokena lortu"
  echo "$RESPONSE" | jq .
  exit 1
fi
echo "OK — Tokena baliozkoa ${EXPIRES}s-z"

echo ""
echo "=== 2. SRMD bidaltzen ==="
SRMD_RESPONSE=$(curl -s -w "\n%{http_code}" -X POST "${DENA_URL}/srmd/" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -d "[{
    \"admin\": { \"id\": \"${ADMIN_ID}\" },
    \"aboutPerson\": { \"id\": \"12345678A\" },
    \"someDataWasUpdatedAt\": \"$(date -u +%Y-%m-%dT%H:%M:%S.000Z)\",
    \"ofType\": { \"id\": \"RECORDS\" },
    \"fromDataOrigin\": \"DEFAULT\"
  }]")

HTTP_CODE=$(echo "$SRMD_RESPONSE" | tail -1)
BODY=$(echo "$SRMD_RESPONSE" | head -n -1)

if [ "$HTTP_CODE" = "200" ]; then
  echo "OK — SRMD zuzen bidalia"
  echo "$BODY" | jq .
else
  echo "ERROREA — HTTP ${HTTP_CODE}"
  echo "$BODY" | jq .
  exit 1
fi

echo ""
echo "=== Emaitza ==="
echo "Tokena: OK (${EXPIRES}s)"
echo "SRMD: OK ($(echo "$BODY" | jq -r '.receivedItemsCount') elementu prozesatu)"
echo ""
echo "Hurrengo urratsa: DENA-k zure /api/retrieveData endpointera deituko du"
echo "pertsonak aplikazioa irekitzen duenean."
```

### Erabilera

```bash
export DENA_CLIENT_ID="zure-client-id"
export DENA_CLIENT_SECRET="zure-client-secret"
chmod +x dena-e2e-test.sh
./dena-e2e-test.sh
```

---

## Zikloaren diagrama osoa

``` mermaid
sequenceDiagram
    autonumber
    participant Admin as Zure Administrazioa
    participant IDP as Keycloak DENA
    participant DENA as DENA-CORE
    participant App as DENA-APP (pertsona)

    rect rgb(232,245,233)
    Note over Admin,DENA: 1. fasea: Aldaketak jakinarazi
    Admin->>IDP: POST /token
    IDP-->>Admin: access_token
    Admin->>DENA: POST /srmd + Bearer token
    DENA-->>Admin: 200 OK
    end

    rect rgb(227,242,253)
    Note over App,DENA: 2. fasea: Pertsonak kontsultatzen du
    App->>DENA: Berrikuntzarik ba al dago?
    DENA-->>App: Bai, RECORDS zure administrazioan
    end

    rect rgb(255,243,224)
    Note over DENA,Admin: 3. fasea: DENA-k datuak berreskuratzen ditu
    DENA->>Admin: POST /api/retrieveData
    Admin-->>DENA: 200 OK + dataItems[]
    DENA-->>App: Datu eguneratuak
    end
```

---

## Hurrengo urratsak

| Zer egin | Non |
|----------|-----|
| Data-Retrieve osoa inplementatu | [:octicons-arrow-right-24: Inplementazio-gida](../semantica/data-retrieve/guia-implementacion.md) |
| Datu-mota gehiago gehitu (hitzorduak, ordainketak...) | [:octicons-arrow-right-24: Datu-motak](../semantica/data-retrieve/index.md) |
| Person-Sync inplementatu | [:octicons-arrow-right-24: Person-Sync](../operativas/person-sync.md) |
| DevTools-ekin probatu | [:octicons-arrow-right-24: DevTools](../devtools/index.md) |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
