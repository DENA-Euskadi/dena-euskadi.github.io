# Semántica Base

## Semantica servicios REST

Todas las peticiones y respuestas de los servicios REST se realizaran con una estructura base igual, que contendra información de contexto, así como los datos concretos de dicha petición o respuesta. La estructura es la siguiente:

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
    ROOT["<b>REST Message</b>"]
    
    ROOT --> CONTEXT["<b>Context</b>"]
    
    ROOT --> DATA["<b>Data</b><br/><i>payload</i>"]
    
    CONTEXT --> MESSAGE_TYPE["<b>messageType</b><br/>String"]
    CONTEXT --> DATA_TYPE["<b>dataType</b><br/>DataTypeRef"]
    CONTEXT --> MESSAGE_CORRELATION_ID["<b>messageCorrelationId</b><br/>UUID"]
    CONTEXT --> FLOW_DIRECTION["<b>flowDirection</b><br/>REQUEST/RESPONSE"]
    CONTEXT --> ORIGIN_PARTY_ID["<b>originPartyId</b><br/>String"]
    CONTEXT --> DESTINATION_PARTY_ID["<b>destinationPartyId</b><br/>String"]
    CONTEXT --> SUBJECT_PERSON["<b>subjectPerson</b><br/>PersonRef"]
    CONTEXT --> ADMINISTRATION["<b>administration</b><br/>OrgAdminRef"]
    CONTEXT --> INTEROP_ROUTE_DATA["<b>interopRouteData</b><br/>Array"]
    
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
|-------|-------------|
| 🟡 Amarillo | Estructura raíz (REST Message) |
| 🔵 Azul claro | Objetos principales (Context y Data) |
| 🔴 Rojo claro | Campos primitivos y referencias |
| 🟣 Violeta | Arrays |

## Ejemplo JSON

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
    "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36",
    "originPartyId": "DENA_POSTMAN",
    "destinationPartyId": "DENA_INTEROP",
    "clientDeviceOid": "59d5b0b0-5662-4152-b2ed-131aa0fb2608",
    "dataType": { "dataTypeId": "RECORDS" },
    "subjectPerson": { "personId": "12345678A" },
    "administration": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" }
  },
  "data": {
    <payload>
  }
}
```

| Campo     | Tipo                | Obligatorio | Descripción |
|-----------|---------------------|-------------|-------------|
| `context` | [Context](#context) | ✅          | Objeto de contexto de la petición |
| `data`    | `Objeto`            | ✅          | Payload de la petición o datos de la respuesta |

## Context

| Campo                  | Tipo           | Obligatorio | Descripción |
|------------------------|----------------|-------------|-------------|
| `interopRouteData`     | `Objeto`       | ✅          | Listado con los ids de cada componente por los que ha pasado la petición, y el timestamp que indica en que momento del tiempo llego a cada componente la llamada |
| `messageCorrelationId` | `String(UUID)` | ✅          | Id de correlación para asociar todas las llamadas que se realicen debido a una petición, en formato UUID |
| `messageType`          | `String`       | ✅          | Tipo de mensaje enviado en el objecto "data" |
| `flowDirection`        | `String`       | ✅          | Indica si el mensaje es una petición (REQUEST) o respuesta (RESPONSE) |
| `userAgent`            | `String`       | ✅          | User Agent del dispositivo en el que se origina la petición |
| `originPartyId`        | `String`       | ✅          | Identificador del origen de la petición. Ej: DENA_WEBAPP |
| `destinationPartyId`   | `String`       | ✅          | Identificador del destino de la petición. Ej: DENA_INTEROP |
| `clientDeviceOid`      | `String`       | ❌          | Oid del dispositivo desde el que se inicia la petición |
| `dataType`             | [DataTypeRef](./modelo/data-type-ref.md) | ❌          | Tipo de dato semantico que solicita o envía la petición, cuando sea aplicable |
| `subjectPerson`        | [PersonRef](./modelo/person-ref.md) | ❌          | Referencia a la persona sobre la que se solicitan o envían datos, cuando sea aplicable |
| `administration`       | [OrgAdminRef](./modelo/org-admin-ref.md) | ❌          | Referencia a la administración a la que se solicitan datos, cuando sea aplicable |

---

## Modelos comunes

A continuación se detallan los modelos comunes utilizados en varios servicios REST:

| Modelo | Descripción |
|--------|-------------|
| [DataTypeRef](./modelo/data-type-ref.md) | Referencia a un tipo de dato |
| [OrgAdminRef](./modelo/org-admin-ref.md) | Referencia a una administración |
| [PersonRef](./modelo/person-ref.md) | Referencia a una persona |
| [LanguageTexts](./modelo/language-texts.md) | Mensajes o textos en multiples idiomas |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
