# :material-play-circle-outline: End-to-End Example

A complete flow from obtaining credentials to receiving data, step by step with `curl`.

!!! info "Prerequisites"
    - Have `client_id` and `client_secret` provided by the DENA team
    - HTTPS connectivity to the DENA PRE environment
    - `curl` and `jq` installed (or any HTTP client)

---

## Complete flow

``` mermaid
sequenceDiagram
    autonumber
    participant Admin as Your System
    participant IDP as Keycloak DENA
    participant DENA as DENA-CORE

    Admin->>IDP: 1. POST /token (client_credentials)
    IDP-->>Admin: 2. access_token (JWT, 5 min)
    Admin->>DENA: 3. POST /srmd + Bearer token
    DENA-->>Admin: 4. 200 OK (processed SRMDs)
```

This example shows the **Administration → DENA** flow (sending Metadata-Sync). The **DENA → Administration** flow (Data-Retrieve) is initiated automatically by DENA — you only need to expose the endpoint.

---

## Step 1: Obtain an OAuth2 token

```bash
# Configuration variables
KEYCLOAK_URL="https://api-batera.pre.dena.eus/auth"
REALM="DenaAuthAdmins"
CLIENT_ID="your-client-id"
CLIENT_SECRET="your-client-secret"

# Obtain token
TOKEN=$(curl -s -X POST \
  "${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}" \
  | jq -r '.access_token')

echo "Token obtained: ${TOKEN:0:20}..."
```

### Expected response

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "expires_in": 300,
  "token_type": "Bearer",
  "scope": "email profile"
}
```

!!! warning "The token expires in 5 minutes"
    Renew the token before it expires. It is recommended to renew it when ~60 seconds remain (leeway).

---

## Step 2: Send Metadata-Sync (SRMD)

With the obtained token, notify DENA that there is new data for a person:

```bash
DENA_URL="https://api-batera.pre.dena.eus"

curl -s -X POST "${DENA_URL}/srmd/" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '[
    {
      "admin": { "id": "YOUR-ADMIN-ID" },
      "aboutPerson": { "id": "12345678A" },
      "someDataWasUpdatedAt": "2026-08-24T10:30:00.000Z",
      "ofType": { "id": "RECORDS" },
      "fromDataOrigin": "DEFAULT"
    }
  ]' | jq .
```

### Expected response (200 OK)

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 1,
  "processedOK": [
    {
      "admin": { "id": "YOUR-ADMIN-ID" },
      "aboutPerson": { "id": "12345678A" },
      "ofType": { "id": "RECORDS" }
    }
  ],
  "processedNOK": []
}
```

!!! success "Done"
    DENA now knows that you have updated records data for person `12345678A`. When the person opens the app, DENA will call your Data-Retrieve endpoint to obtain that data.

---

## Step 3: Expose your Data-Retrieve endpoint

DENA will call your system when the person needs to view their data. Your endpoint must:

1. Receive a `POST` with the `personId` and `dataTypeId`
2. Query your database
3. Return the data in DENA format

### Request you will receive from DENA

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "RECORDS" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "REQUEST",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": {}
}
```

### Response you must return

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "RECORDS" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": {
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

## Step 4: Verify with curl (simulating DENA)

To test your endpoint locally before the real integration:

```bash
curl -s -X POST http://localhost:8080/api/retrieveData \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '{
    "context": {
      "messageType": "PERSON_FETCH_DATA",
      "dataType": { "dataTypeId": "RECORDS" },
      "messageCorrelationId": "test-001",
      "flowDirection": "REQUEST",
      "subjectPerson": { "personId": "12345678A" }
    },
    "data": {}
  }' | jq .
```

### Validation checklist

- [ ] Returns HTTP 200 with `code: "OK"`
- [ ] `dataItems` is an array (empty if there is no data)
- [ ] Each item has `oid`, `id`, `lastChangedAt`
- [ ] Texts include `SPANISH` and `BASQUE`
- [ ] `messageCorrelationId` from the request is preserved in the response
- [ ] `flowDirection` is `RESPONSE`
- [ ] Electronic office URLs included per language
- [ ] Response time < 30 seconds

---

## Complete script

A Bash script that executes the entire end-to-end flow:

```bash
#!/bin/bash
# dena-e2e-test.sh — End-to-end test against DENA PRE
set -e

# === CONFIGURATION ===
KEYCLOAK_URL="https://api-batera.pre.dena.eus/auth"
REALM="DenaAuthAdmins"
CLIENT_ID="${DENA_CLIENT_ID:?Set DENA_CLIENT_ID}"
CLIENT_SECRET="${DENA_CLIENT_SECRET:?Set DENA_CLIENT_SECRET}"
DENA_URL="https://api-batera.pre.dena.eus"
ADMIN_ID="YOUR-ADMIN-ID"

echo "=== 1. Obtaining token ==="
RESPONSE=$(curl -s -X POST \
  "${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}")

TOKEN=$(echo "$RESPONSE" | jq -r '.access_token')
EXPIRES=$(echo "$RESPONSE" | jq -r '.expires_in')

if [ "$TOKEN" = "null" ] || [ -z "$TOKEN" ]; then
  echo "ERROR: Could not obtain token"
  echo "$RESPONSE" | jq .
  exit 1
fi
echo "OK — Token valid for ${EXPIRES}s"

echo ""
echo "=== 2. Sending SRMD ==="
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
  echo "OK — SRMD sent successfully"
  echo "$BODY" | jq .
else
  echo "ERROR — HTTP ${HTTP_CODE}"
  echo "$BODY" | jq .
  exit 1
fi

echo ""
echo "=== Result ==="
echo "Token: OK (${EXPIRES}s)"
echo "SRMD: OK ($(echo "$BODY" | jq -r '.receivedItemsCount') items processed)"
echo ""
echo "Next step: DENA will call your /api/retrieveData endpoint"
echo "when the person opens the app."
```

### Usage

```bash
export DENA_CLIENT_ID="your-client-id"
export DENA_CLIENT_SECRET="your-client-secret"
chmod +x dena-e2e-test.sh
./dena-e2e-test.sh
```

---

## Complete cycle diagram

``` mermaid
sequenceDiagram
    autonumber
    participant Admin as Your Administration
    participant IDP as Keycloak DENA
    participant DENA as DENA-CORE
    participant App as DENA-APP (person)

    rect rgb(232,245,233)
    Note over Admin,DENA: Phase 1: Notify changes
    Admin->>IDP: POST /token
    IDP-->>Admin: access_token
    Admin->>DENA: POST /srmd + Bearer token
    DENA-->>Admin: 200 OK
    end

    rect rgb(227,242,253)
    Note over App,DENA: Phase 2: Person queries
    App->>DENA: Any updates?
    DENA-->>App: Yes, RECORDS in your admin
    end

    rect rgb(255,243,224)
    Note over DENA,Admin: Phase 3: DENA retrieves data
    DENA->>Admin: POST /api/retrieveData
    Admin-->>DENA: 200 OK + dataItems[]
    DENA-->>App: Updated data
    end
```

---

## Next steps

| What to do | Where |
|-----------|-------|
| Implement complete Data-Retrieve | [:octicons-arrow-right-24: Implementation guide](../semantica/data-retrieve/guia-implementacion.md) |
| Add more data types (appointments, payments...) | [:octicons-arrow-right-24: Data types](../semantica/data-retrieve/index.md) |
| Implement Person-Sync | [:octicons-arrow-right-24: Person-Sync](../operativas/person-sync.md) |
| Test with DevTools | [:octicons-arrow-right-24: DevTools](../devtools/index.md) |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
