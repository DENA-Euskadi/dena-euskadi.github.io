# :material-play-circle-outline: Ejemplo End-to-End

Un flujo completo desde obtener credenciales hasta recibir datos, paso a paso con `curl`.

!!! info "Prerequisitos"
    - Tener `client_id` y `client_secret` proporcionados por el equipo DENA
    - Conectividad HTTPS hacia el entorno PRE de DENA
    - `curl` y `jq` instalados (o cualquier cliente HTTP)

---

## Flujo completo

``` mermaid
sequenceDiagram
    autonumber
    participant Admin as Tu Sistema
    participant IDP as Keycloak DENA
    participant DENA as DENA-CORE

    Admin->>IDP: 1. POST /token (client_credentials)
    IDP-->>Admin: 2. access_token (JWT, 5 min)
    Admin->>DENA: 3. POST /srmd + Bearer token
    DENA-->>Admin: 4. 200 OK (SRMD procesados)
```

Este ejemplo muestra el flujo **Administración → DENA** (enviar Metadata-Sync). El flujo **DENA → Administración** (Data-Retrieve) lo inicia DENA automáticamente — tú solo expones el endpoint.

---

## Paso 1: Obtener un token OAuth2

```bash
# Variables de configuración
KEYCLOAK_URL="https://api-batera.pre.dena.eus/auth"
REALM="DenaAuthAdmins"
CLIENT_ID="tu-client-id"
CLIENT_SECRET="tu-client-secret"

# Obtener token
TOKEN=$(curl -s -X POST \
  "${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}" \
  | jq -r '.access_token')

echo "Token obtenido: ${TOKEN:0:20}..."
```

### Respuesta esperada

```json
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "expires_in": 300,
  "token_type": "Bearer",
  "scope": "email profile"
}
```

!!! warning "El token expira en 5 minutos"
    Renueva el token antes de que expire. Se recomienda renovarlo cuando queden ~60 segundos (leeway).

---

## Paso 2: Enviar Metadata-Sync (SRMD)

Con el token obtenido, notifica a DENA que hay datos nuevos para una persona:

```bash
DENA_URL="https://api-batera.pre.dena.eus"

curl -s -X POST "${DENA_URL}/srmd/" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json" \
  -d '[
    {
      "admin": { "id": "TU-ADMIN-ID" },
      "aboutPerson": { "id": "12345678A" },
      "someDataWasUpdatedAt": "2026-08-24T10:30:00.000Z",
      "ofType": { "id": "RECORDS" },
      "fromDataOrigin": "DEFAULT"
    }
  ]' | jq .
```

### Respuesta esperada (200 OK)

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 1,
  "processedOK": [
    {
      "admin": { "id": "TU-ADMIN-ID" },
      "aboutPerson": { "id": "12345678A" },
      "ofType": { "id": "RECORDS" }
    }
  ],
  "processedNOK": []
}
```

!!! success "Hecho"
    DENA ahora sabe que tienes datos actualizados de expedientes para la persona `12345678A`. Cuando la persona abra la app, DENA llamará a tu endpoint Data-Retrieve para obtener esos datos.

---

## Paso 3: Exponer tu endpoint Data-Retrieve

DENA llamará a tu sistema cuando la persona necesite ver sus datos. Tu endpoint debe:

1. Recibir un `POST` con el id de la persona (`subjectPerson.id`) y el tipo de dato (`dataType.id`)
2. Consultar tu base de datos
3. Devolver los datos en formato DENA

### Request que recibirás de DENA

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

### Response que debes devolver

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

## Paso 4: Verificar con curl (simulando DENA)

Para probar tu endpoint localmente antes de la integracion real:

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

### Checklist de validacion

- [ ] Devuelve HTTP 200 con `code: "OK"`
- [ ] `dataItems` es un array (vacio si no hay datos)
- [ ] Cada item tiene `oid`, `id`, `lastChangedAt`
- [ ] Textos incluyen `SPANISH` y `BASQUE`
- [ ] `context.message.correlationId` de la request se mantiene en la response
- [ ] URLs de sede electronica incluidas por idioma
- [ ] Tiempo de respuesta < 30 segundos

---

## Script completo

Un script Bash que ejecuta todo el flujo end-to-end:

```bash
#!/bin/bash
# dena-e2e-test.sh — Test end-to-end contra DENA PRE
set -e

# === CONFIGURACION ===
KEYCLOAK_URL="https://api-batera.pre.dena.eus/auth"
REALM="DenaAuthAdmins"
CLIENT_ID="${DENA_CLIENT_ID:?Configura DENA_CLIENT_ID}"
CLIENT_SECRET="${DENA_CLIENT_SECRET:?Configura DENA_CLIENT_SECRET}"
DENA_URL="https://api-batera.pre.dena.eus"
ADMIN_ID="TU-ADMIN-ID"

echo "=== 1. Obteniendo token ==="
RESPONSE=$(curl -s -X POST \
  "${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=client_credentials&client_id=${CLIENT_ID}&client_secret=${CLIENT_SECRET}")

TOKEN=$(echo "$RESPONSE" | jq -r '.access_token')
EXPIRES=$(echo "$RESPONSE" | jq -r '.expires_in')

if [ "$TOKEN" = "null" ] || [ -z "$TOKEN" ]; then
  echo "ERROR: No se pudo obtener token"
  echo "$RESPONSE" | jq .
  exit 1
fi
echo "OK — Token valido por ${EXPIRES}s"

echo ""
echo "=== 2. Enviando SRMD ==="
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
  echo "OK — SRMD enviado correctamente"
  echo "$BODY" | jq .
else
  echo "ERROR — HTTP ${HTTP_CODE}"
  echo "$BODY" | jq .
  exit 1
fi

echo ""
echo "=== Resultado ==="
echo "Token: OK (${EXPIRES}s)"
echo "SRMD: OK ($(echo "$BODY" | jq -r '.receivedItemsCount') items procesados)"
echo ""
echo "Siguiente paso: DENA llamara a tu endpoint /api/retrieveData"
echo "cuando la persona abra la app."
```

### Uso

```bash
export DENA_CLIENT_ID="tu-client-id"
export DENA_CLIENT_SECRET="tu-client-secret"
chmod +x dena-e2e-test.sh
./dena-e2e-test.sh
```

---

## Diagrama completo del ciclo

``` mermaid
sequenceDiagram
    autonumber
    participant Admin as Tu Administracion
    participant IDP as Keycloak DENA
    participant DENA as DENA-CORE
    participant App as DENA-APP (persona)

    rect rgb(232,245,233)
    Note over Admin,DENA: Fase 1: Notificar cambios
    Admin->>IDP: POST /token
    IDP-->>Admin: access_token
    Admin->>DENA: POST /srmd + Bearer token
    DENA-->>Admin: 200 OK
    end

    rect rgb(227,242,253)
    Note over App,DENA: Fase 2: Persona consulta
    App->>DENA: Hay novedades?
    DENA-->>App: Si, RECORDS en tu admin
    end

    rect rgb(255,243,224)
    Note over DENA,Admin: Fase 3: DENA recupera datos
    DENA->>Admin: POST /api/retrieveData
    Admin-->>DENA: 200 OK + dataItems[]
    DENA-->>App: Datos actualizados
    end
```

---

## Siguientes pasos

| Que hacer | Donde |
|-----------|-------|
| Implementar Data-Retrieve completo | [:octicons-arrow-right-24: Guia de implementacion](../semantica/data-retrieve/guia-implementacion.md) |
| Anadir mas tipos de dato (citas, pagos...) | [:octicons-arrow-right-24: Tipos de dato](../semantica/data-retrieve/index.md) |
| Implementar Person-Sync | [:octicons-arrow-right-24: Person-Sync](../operativas/person-sync.md) |
| Probar con DevTools | [:octicons-arrow-right-24: DevTools](../devtools/index.md) |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
