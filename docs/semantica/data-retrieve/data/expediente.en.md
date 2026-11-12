# :material-folder-open: Record (Procedure Record)

> - **Version:** `v{{ dena.version }}`
> - **Date:** {{ dena.date }}
> - **Type ID (DataTypeID):** `ADMINISTRATIVE_RECORD`
> - **Test:** [DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java)
> - **Code:** [DN00AdmistrativeServiceProcedureRecord.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java)

## Description

Represents an administrative record linked to a service and procedure. It is the central object of the model: notifications, registrations and payments depend on it.

> See also: [campos-comunes.md](./campos-comunes.md) for inherited fields (`oid`, `id`, `lastChangedAt`, `urls`, `originAdmin`, `aboutPerson`)

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
    EXP["<b>Record</b><br/><i>administrativeServiceProcedureRecord</i>"]

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

    click SRV "../servicio-administrativo/" "View Administrative Service"
    click PROC "../servicio-administrativo/" "View Procedure"
```

| Colour | Meaning |
|--------|---------|
| 🟠 Orange | Main object (Record) |
| ⚪ Grey | Hierarchical context (Service / Procedure) |
| 🟣 Violet | Enums / states |
| 🔵 Light blue | Data fields |

---

## 🔗 Source code

| Class | Repository |
|-------|------------|
| DN00AdmistrativeServiceProcedureRecord | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java) |
| DN00AdmistrativeServiceProcedureApplicationData | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureApplicationData.java) |
| DN00AdmistrativeServiceProcedureRecordData | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecordData.java) |
| DN00AdministrativeServiceInterestedParty | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceInterestedParty.java) |
| DN00AdministrativeServiceProcedureRecordState | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedureRecordState.java) |
| DN00AdministrativeServiceProcedureRecordStateCode | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedureRecordStateCode.java) |

---

## 🧪 Tests and examples

| Test | Repository |
|------|------------|
| DN00RecordTest | [View test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |
| DN00RecordStatusTest | [View test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdministrativeServiceProcedureRecordState.java) |
| DN00RecordIDTest | [View test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |

---

## JSON attributes

Fields marked with <span class="field-badge--common">common</span> are **inherited** from [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java) and are present in all data-retrieve types. See [campos-comunes.md](./campos-comunes.md).

| Field | Type | Mandatory | Example | Description |
|-------|------|:---------:|---------|-------------|
| `type` | `String` | ✅ | `"administrativeServiceProcedureRecord"` | Polymorphic discriminator |
| `oid` <span class="field-badge--common">common</span> | `String` | ✅ | `"EXP-OID-001"` | Unique technical identifier |
| `id` <span class="field-badge--common">common</span> | `String` | ✅ | `"EXP-2024-00123"` | Business identifier |
| `lastChangedAt` <span class="field-badge--common">common</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Last change of the data at the origin. DENA-CORE uses it to compute the NEW/UPDATED/UNCHANGED state |
| `service` | `Object` | ✅ | *(see [servicio-administrativo.md](./servicio-administrativo.md) · [`DN00AdmistrativeService`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeService.java))* | Administrative service |
| `procedure` | `Object` | ✅ | *(see [servicio-administrativo.md](./servicio-administrativo.md) · [`DN00AdministrativeServiceProcedure`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedure.java))* | Procedure |
| `applicationData` | `Object` | ❌ | *(see [applicationData](#applicationdata))* | Application submission data |
| `applicationData.date` | `String` (ISO 8601) | ❌ | `"2024-03-14T09:00:00Z"` | Application submission date |
| `applicationData.ledgerId` | `String` | ❌ | `"REG-2024-00123"` | Assigned register entry number |
| `recordData` | `Object` | ❌ | *(see [recordData](#recorddata))* | Record data (state, dates, interested party) |
| `recordData.createdAt` | `String` (ISO 8601) | ❌ | `"2024-03-15T10:30:00Z"` | Record creation date |
| `recordData.lastModifiedAt` | `String` (ISO 8601) | ❌ | `"2024-06-01T14:00:00Z"` | Last modification date |
| `recordData.state` | `Object` | ❌ | `{"stateCode":"IN_PROGRESS"}` | Record state |
| `recordData.state.stateCode` | `String` | ❌ | `"IN_PROGRESS"` | State code |
| `recordData.state.description` | `LanguageTexts` | ❌ | `{"SPANISH":"En tramitación"}` | Multilingual state description |
| `recordData.interestedParty` | `Object` | ❌ | `{"personId":"12345678A","displayName":"Juan García"}` | Interested party in the record |
| `recordData.interestedParty.personId` | `String` | ❌ | `"12345678A"` | NIF/DNI of the interested party |
| `recordData.interestedParty.displayName` | `String` | ❌ | `"Juan García"` | Name of the interested party |
| `descriptionByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"Licencia apertura"}` | Record description |
| `urls` <span class="field-badge--common">common</span> | `Array` | ❌ (recommended) | *(see [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | Access URLs to the e-government portal |

---

## States (`state.stateCode`)

| Code | Description |
|------|-------------|
| `REGISTERED_PENDING_TO_BE_OPENED` | Registered, pending opening |
| `OPENED` | Opened |
| `IN_PROGRESS` | In progress |
| `WAITING_FOR_INTERESTED_PARTY_RESPONSE` | Awaiting response from the interested party |
| `WAITING_FOR_OTHER_ORG_WORK` | Awaiting another organisation |
| `CLOSED` | Closed |

---

## JSON example

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

<p class="common-fields-legend"><span class="swatch"></span> Highlighted lines are <strong>common</strong> fields inherited from the base class (see <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

## Relationship with other objects

The following objects reference the record via `record` (previously `procedureRecord`):

- Notification (`administrativeNotice`)
- Register Ledger (`administrativeLedger`)
- Payment (`oneOffPayment`, `directDebitPayment`)
- Economic benefit (`economicBenefit`)
- Person credential (`personCredential`)
- Appointment (`scheduleItem`) — `record` field is optional in appointments

---

## Validation rules

- `service` and `procedure` must include at least `nameByLanguage` with texts in `SPANISH` and `BASQUE`.
- `recordData.createdAt`, if included, must be a valid ISO 8601 date.
- If `recordData.lastModifiedAt` is included, it must be ≥ `recordData.createdAt`.
- See [validaciones.md](../validaciones.md) for full rules.


<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
