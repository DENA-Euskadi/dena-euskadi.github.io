# :material-shape: Semántica Base

> **Versión:** `v{{ dena.version }}` · **Fecha:** {{ dena.date }}

---

## Estructura de los servicios REST

Todas las peticiones y respuestas de los servicios REST comparten una estructura base con información de **contexto** y **datos**:

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#000000"
    primaryBorderColor: "#CCCCCC"
    lineColor: "#4A4A4A"
    fontSize: "12px"
---
flowchart LR
    ROOT["REST Message"]

    ROOT --> CONTEXT["Context"]
    ROOT --> DATA["Data<br/><i>payload</i>"]

    CONTEXT --> MESSAGE_TYPE["messageType<br/>String"]
    CONTEXT --> DATA_TYPE["dataType<br/>DataTypeRef"]
    CONTEXT --> MESSAGE_CORRELATION_ID["messageCorrelationId<br/>UUID"]
    CONTEXT --> FLOW_DIRECTION["flowDirection<br/>REQUEST/RESPONSE"]
    CONTEXT --> ORIGIN_PARTY_ID["originPartyId<br/>String"]
    CONTEXT --> DESTINATION_PARTY_ID["destinationPartyId<br/>String"]
    CONTEXT --> SUBJECT_PERSON["subjectPerson<br/>PersonRef"]
    CONTEXT --> ADMINISTRATION["administration<br/>OrgAdminRef"]
    CONTEXT --> INTEROP_ROUTE_DATA["interopRouteData<br/>Array"]

    INTEROP_ROUTE_DATA --> IRDITEM["denaComponentId<br/>timestamp"]

    style ROOT fill:#fff2cc,stroke:#d6b656,color:#000000,rx:8,ry:8
    style CONTEXT fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:8,ry:8
    style DATA fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:8,ry:8
    style MESSAGE_TYPE fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style DATA_TYPE fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style MESSAGE_CORRELATION_ID fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style FLOW_DIRECTION fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style ORIGIN_PARTY_ID fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style DESTINATION_PARTY_ID fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style SUBJECT_PERSON fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style ADMINISTRATION fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
    style INTEROP_ROUTE_DATA fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style IRDITEM fill:#f8cecc,stroke:#b85450,color:#000000,rx:6,ry:6
```

| Color | Significado |
|---|---|
| :yellow_circle: Amarillo | Estructura raíz (REST Message) |
| :blue_circle: Azul claro | Objetos principales (Context y Data) |
| :red_circle: Rojo claro | Campos primitivos y referencias |
| :purple_circle: Violeta | Arrays |

---

## Ejemplo JSON

!!! example "Estructura base de una petición"

    ```json
    {
      "context": {
        "interopRouteData": [
          {
            "denaComponentId": "DENA_POSTMAN",
            "denaTS": 1779382284684
          }
        ],
        "messageCorrelationId": "59d5b0b0-5662-4152-b2ed-131aa0fb2608",
        "messageType": "CLIENT_LOGIN",
        "flowDirection": "REQUEST",
        "userAgent": "Mozilla/5.0 ...",
        "originPartyId": "DENA_POSTMAN",
        "destinationPartyId": "DENA_INTEROP",
        "clientDeviceOid": "59d5b0b0-5662-4152-b2ed-131aa0fb2608",
        "dataType": { "dataTypeId": "RECORDS" },
        "subjectPerson": { "personId": "12345678A" },
        "administration": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" }
      },
      "data": {
        ...
      }
    }
    ```

---

## Campos del mensaje

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `context` | [Context](#context) | :material-check: | Objeto de contexto de la petición |
| `data` | `Objeto` | :material-check: | Payload de la petición o datos de la respuesta |

---

## Context

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `interopRouteData` | `Array` | :material-check: | Listado de componentes por los que ha pasado la petición con su timestamp |
| `messageCorrelationId` | `String(UUID)` | :material-check: | Id de correlación para asociar todas las llamadas derivadas de una petición |
| `messageType` | `String` | :material-check: | Tipo de mensaje enviado en el objeto `data` |
| `flowDirection` | `String` | :material-check: | Indica si es petición (`REQUEST`) o respuesta (`RESPONSE`) |
| `userAgent` | `String` | :material-check: | User Agent del dispositivo origen |
| `originPartyId` | `String` | :material-check: | Identificador del origen (ej: `DENA_WEBAPP`) |
| `destinationPartyId` | `String` | :material-check: | Identificador del destino (ej: `DENA_INTEROP`) |
| `clientDeviceOid` | `String` | :material-close: | OID del dispositivo origen |
| `dataType` | [DataTypeRef](./modelo/data-type-ref.md) | :material-close: | Tipo de dato semántico solicitado |
| `subjectPerson` | [PersonRef](./modelo/person-ref.md) | :material-close: | Persona sobre la que se solicitan datos |
| `administration` | [OrgAdminRef](./modelo/org-admin-ref.md) | :material-close: | Administración destino |

---

## Estructura completa del mensaje

Un mensaje DENA tiene la siguiente estructura general:

```json
{
  "context": { ... },       // Metadatos de contexto (obligatorio)
  "protocol": { ... },      // Información de protocolo (opcional)
  "consent": { ... },       // Base habilitante (solo en Data-Retrieve)
  "status": { ... },        // Estado de respuesta (solo en responses)
  "data": { ... }           // Payload (obligatorio)
}
```

| Bloque | Presencia | Descripción |
|---|---|---|
| `context` | Siempre | Identificación, correlación, tipo de operación |
| `protocol` | Cuando necesario | URLs, timeouts, hashes, tokens |
| `consent` | Solo Data-Retrieve | Referencia al consentimiento/habilitación normativa |
| `status` | Solo respuestas | Resultado del procesamiento |
| `data` | Siempre | Datos de la petición o respuesta |

---

## HTTP Headers

Todas las llamadas HTTP incluyen cabeceras estándar y personalizadas para seguridad, trazabilidad y versionado.

[:octicons-arrow-right-24: Ver HTTP Headers](./http-headers.md)

---

## Protocol (DENAProtocol)

Información de protocolo: URLs de callback, timeouts, hashes.

[:octicons-arrow-right-24: Ver DENAProtocol](./modelo/protocol.md)

---

## Consent (DENAConsent)

Referencia a la base habilitante (consentimiento/habilitación normativa) que respalda una petición Data-Retrieve.

[:octicons-arrow-right-24: Ver DENAConsent](./modelo/consent.md)

---

## Status (Respuesta)

Información sobre el resultado del procesamiento en mensajes de respuesta.

[:octicons-arrow-right-24: Ver Status](./modelo/status.md)

---

## Consentimientos

Principios, ciclo de vida y API del sistema de consentimientos de DENA.

[:octicons-arrow-right-24: Ver Consentimientos](./consentimientos.md)

---

## Modelos comunes

!!! info "Tipos de dato fundacionales"
    Para los tipos de dato básicos usados en todo DENA (Boolean, Numbers, Dates, Ranges, UIDs, URLs, LanguageTexts, Money, Hash, UserAgent...) consulta [:octicons-arrow-right-24: Modelo de Datos Base](../../arquitectura/tipos-dato-base.md)

<div class="grid cards" markdown>

-   :material-cube-outline:{ .lg .middle } **DenaObjectRef**

    ---

    Tipo base: OID, timestamps, URL de cualquier objeto DENA.

    [:octicons-arrow-right-24: Ver modelo](./modelo/object-ref.md)

-   :material-tag:{ .lg .middle } **DataTypeRef**

    ---

    Referencia a un tipo de dato gestionado por DENA.

    [:octicons-arrow-right-24: Ver modelo](./modelo/data-type-ref.md)

-   :material-domain:{ .lg .middle } **OrgAdminRef**

    ---

    Referencia a una administración (orgId, DIR3, OID...).

    [:octicons-arrow-right-24: Ver modelo](./modelo/org-admin-ref.md)

-   :material-account:{ .lg .middle } **PersonRef**

    ---

    Referencia a una persona registrada (personId, OID...).

    [:octicons-arrow-right-24: Ver modelo](./modelo/person-ref.md)

-   :material-translate:{ .lg .middle } **LanguageTexts**

    ---

    Textos en múltiples idiomas (castellano, euskera, inglés).

    [:octicons-arrow-right-24: Ver modelo](./modelo/language-texts.md)

-   :material-message-text:{ .lg .middle } **Tipos de Mensaje**

    ---

    FlowDirection, MessageType, RouteDataItem, PersonAndConsentGiven.

    [:octicons-arrow-right-24: Ver modelo](./modelo/message-types.md)

-   :material-cellphone-link:{ .lg .middle } **UserAgent**

    ---

    Formato del User-Agent según origen del mensaje.

    [:octicons-arrow-right-24: Ver modelo](./modelo/user-agent.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
