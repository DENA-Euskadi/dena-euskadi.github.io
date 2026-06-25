# :material-shape: Semántica Base

> **Versión:** `v0.3.26` · **Fecha:** 2026-06-11

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

## Modelos comunes

<div class="grid cards" markdown>

-   :material-tag:{ .lg .middle } **DataTypeRef**

    ---

    Referencia a un tipo de dato gestionado por DENA.

    [:octicons-arrow-right-24: Ver modelo](./modelo/data-type-ref.md)

-   :material-domain:{ .lg .middle } **OrgAdminRef**

    ---

    Referencia a una administración.

    [:octicons-arrow-right-24: Ver modelo](./modelo/org-admin-ref.md)

-   :material-account:{ .lg .middle } **PersonRef**

    ---

    Referencia a una persona registrada.

    [:octicons-arrow-right-24: Ver modelo](./modelo/person-ref.md)

-   :material-translate:{ .lg .middle } **LanguageTexts**

    ---

    Textos en múltiples idiomas (castellano, euskera, inglés).

    [:octicons-arrow-right-24: Ver modelo](./modelo/language-texts.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
