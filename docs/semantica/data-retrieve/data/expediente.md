# :material-folder-open: Expediente (Procedure Record)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **ID de Tipo (DataTypeID):** `ADMINISTRATIVE_RECORD`
> - **Test:** [DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java)
> - **Código:** [DN00AdmistrativeServiceProcedureRecord.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java)

## Descripción

Representa un expediente administrativo vinculado a un servicio y procedimiento. Es el objeto central del modelo: notificaciones, registros y pagos dependen de él.

> Ver también: [campos-comunes.md](./campos-comunes.md) para campos heredados (`oid`, `id`, `lastChangedAt`, `urls`, `originAdmin`, `aboutPerson`)

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
    EXP["<b>Expediente</b><br/><i>administrativeServiceProcedureRecord</i>"]

    EXP --> SRV["service<br/><i>nameByLanguage · originRef</i>"]
    EXP --> PROC["procedure<br/><i>nameByLanguage · originRef</i>"]
    EXP --> APPDATA["applicationData<br/><i>date · ledgerId</i>"]
    EXP --> RECDATA["recordData<br/><i>createdAt · lastModifiedAt</i>"]
    EXP --> DESC["descriptionByLanguage"]
    EXP --> URLS["urls[]"]

    RECDATA --> STATE["state<br/><i>stateCode · description</i>"]
    RECDATA --> PARTY["interestedParty<br/><i>personId · displayName</i>"]

    style EXP fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style SRV fill:#f5f5f5,stroke:#666666,color:#000000,rx:6,ry:6
    style PROC fill:#f5f5f5,stroke:#666666,color:#000000,rx:6,ry:6
    style APPDATA fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style RECDATA fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style DESC fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style URLS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style STATE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style PARTY fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click SRV "../servicio-administrativo/" "Ver Servicio Administrativo"
    click PROC "../servicio-administrativo/" "Ver Procedimiento"
```

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (Expediente) |
| ⚪ Gris | Contexto jerárquico (Servicio / Procedimiento) |
| 🟣 Violeta | Enums / estados |
| 🔵 Azul claro | Campos de datos |

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00AdmistrativeServiceProcedureRecord | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java) |
| DN00AdmistrativeServiceProcedureApplicationData | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureApplicationData.java) |
| DN00AdmistrativeServiceProcedureRecordData | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecordData.java) |
| DN00AdministrativeServiceInterestedParty | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceInterestedParty.java) |
| DN00AdministrativeServiceProcedureRecordState | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedureRecordState.java) |
| DN00AdministrativeServiceProcedureRecordStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedureRecordStateCode.java) |

---

## 🧪 Tests y ejemplos

| Test | Repositorio |
|------|-------------|
| DN00RecordTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |
| DN00RecordStatusTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdministrativeServiceProcedureRecordState.java) |
| DN00RecordIDTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |

---

## Atributos JSON

Los campos marcados con <span class="field-badge--common">común</span> son **heredados** de [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java) y están presentes en todos los tipos del data-retrieve. Ver [campos-comunes.md](./campos-comunes.md).

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"administrativeServiceProcedureRecord"` | Discriminador polimórfico |
| `oid` <span class="field-badge--common">común</span> | `String` | ✅ | `"EXP-OID-001"` | Identificador técnico único |
| `id` <span class="field-badge--common">común</span> | `String` | ✅ | `"EXP-2024-00123"` | Identificador de negocio |
| `lastChangedAt` <span class="field-badge--common">común</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Última modificación del dato en el origen. DENA-CORE lo usa para calcular el estado NEW/UPDATED/UNCHANGED |
| `service` | `Object` | ✅ | *(ver [servicio-administrativo.md](./servicio-administrativo.md) · [`DN00AdmistrativeService`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeService.java))* | Servicio administrativo |
| `procedure` | `Object` | ✅ | *(ver [servicio-administrativo.md](./servicio-administrativo.md) · [`DN00AdministrativeServiceProcedure`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedure.java))* | Procedimiento |
| `applicationData` | `Object` | ❌ | *(ver [applicationData](#applicationdata))* | Datos de presentación de la solicitud |
| `applicationData.date` | `String` (ISO 8601) | ❌ | `"2024-03-14T09:00:00Z"` | Fecha de presentación de la solicitud |
| `applicationData.ledgerId` | `String` | ❌ | `"REG-2024-00123"` | Número de asiento registral asignado |
| `recordData` | `Object` | ❌ | *(ver [recordData](#recorddata))* | Datos del expediente (estado, fechas, interesado) |
| `recordData.createdAt` | `String` (ISO 8601) | ❌ | `"2024-03-15T10:30:00Z"` | Fecha de creación del expediente |
| `recordData.lastModifiedAt` | `String` (ISO 8601) | ❌ | `"2024-06-01T14:00:00Z"` | Fecha de última modificación |
| `recordData.state` | `Object` | ❌ | `{"stateCode":"IN_PROGRESS"}` | Estado del expediente |
| `recordData.state.stateCode` | `String` | ❌ | `"IN_PROGRESS"` | Código de estado |
| `recordData.state.description` | `LanguageTexts` | ❌ | `{"SPANISH":"En tramitación"}` | Descripción multiidioma del estado |
| `recordData.interestedParty` | `Object` | ❌ | `{"personId":"12345678A","displayName":"Juan García"}` | Interesado en el expediente |
| `recordData.interestedParty.personId` | `String` | ❌ | `"12345678A"` | NIF/DNI del interesado |
| `recordData.interestedParty.displayName` | `String` | ❌ | `"Juan García"` | Nombre del interesado |
| `descriptionByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"Licencia apertura"}` | Descripción del expediente |
| `urls` <span class="field-badge--common">común</span> | `Array` | ❌ (recomendado) | *(ver [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | URLs de acceso a la sede electrónica |

---

## Estados (`state.stateCode`)

| Código | Descripción |
|--------|-------------|
| `REGISTERED_PENDING_TO_BE_OPENED` | Registrado, pendiente de apertura |
| `OPENED` | Abierto |
| `IN_PROGRESS` | En tramitación |
| `WAITING_FOR_INTERESTED_PARTY_RESPONSE` | Pendiente de respuesta del interesado |
| `WAITING_FOR_OTHER_ORG_WORK` | Pendiente de otro organismo |
| `CLOSED` | Cerrado |

---

## Ejemplo JSON

<div class="common-highlight" markdown>

```json hl_lines="3 4 5 6"
{
  "type": "administrativeServiceProcedureRecord",
  "oid": "EXP-OID-001",
  "id": "EXP-2024-00123",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "urls": [
    { "url": "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language": "SPANISH", "tags": ["default"] },
    { "url": "https://egoitza.miadmin.eus/espedientea/EXP-2024-00123", "language": "BASQUE", "tags": ["default"] }
  ],
  "service": {
    "nameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
    "originRef": { "id": "SRV-LIC-ACT" }
  },
  "procedure": {
    "nameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
    "originRef": { "id": "PROC-LIC-APER" }
  },
  "applicationData": {
    "date": "2024-03-14T09:00:00Z",
    "ledgerId": "REG-2024-00123"
  },
  "recordData": {
    "createdAt": "2024-03-15T10:30:00Z",
    "lastModifiedAt": "2024-06-01T14:00:00Z",
    "state": {
      "stateCode": "IN_PROGRESS",
      "description": { "SPANISH": "En tramitación", "BASQUE": "Izapidetzen" }
    },
    "interestedParty": {
      "personId": "12345678A",
      "displayName": "Juan García"
    }
  },
  "descriptionByLanguage": {
    "SPANISH": "Expediente de licencia de apertura",
    "BASQUE": "Irekiera-lizentzia espedientea"
  }
}
```

<p class="common-fields-legend"><span class="swatch"></span> Las líneas resaltadas son los campos <strong>comunes</strong> heredados de la clase base (ver <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

## Relación con otros objetos

Los siguientes objetos referencian al expediente mediante `record` (antes `procedureRecord`):

- Notificación (`administrativeNotice`)
- Libro Registro (`administrativeLedger`)
- Pago (`oneOffPayment`, `directDebitPayment`)
- Beneficio económico (`economicBenefit`)
- Credencial de persona (`personCredential`)
- Cita (`scheduleItem`) — campo `record` es opcional en las citas

---

## Reglas de validación

- `service` y `procedure` deben incluir al menos `nameByLanguage` con textos en `SPANISH` y `BASQUE`.
- `recordData.createdAt`, si se incluye, debe ser una fecha ISO 8601 válida.
- Si se incluye `recordData.lastModifiedAt`, debe ser ≥ `recordData.createdAt`.
- Ver [validaciones.md](../validaciones.md) para reglas completas.


<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
