# :material-book-open-page-variant: Official Registration (Official Registry Record)

> - **Version:** `v{{ dena.version }}`
> - **Date:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForAdministrativeOfficialRegistryRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/registry/DN99DENATestMockObjFactoryForAdministrativeOfficialRegistryRecord.java)
> - **Code:** [DN00AdministrativeOfficialRegistryRecord.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegistryModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegistryRecord.java)

## Description

Represents an inbound or outbound registry entry in an official register, linked to a record.

> See also: [campos-comunes.md](./campos-comunes.md) for inherited fields (`oid`, `id`, `urls`, `originAdminRef`, `aboutPersonRef`)

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
    REG["<b>Official Registration</b><br/><i>administrativeOfficialRegisterRecord</i>"]

    REG --> EXP["procedureRecord<br/><i>oid · id</i>"]
    REG --> DATE["registeredAt"]
    REG --> SUBJECT["subjectByLanguage"]
    REG --> STATE["state<br/><i>stateCode · description</i>"]
    REG --> URLS["urls[]"]

    style REG fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style EXP fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style DATE fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style SUBJECT fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style STATE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style URLS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click EXP "../expediente/" "View Record"
```

| Colour | Meaning |
|--------|---------|
| 🟠 Orange | Main object (Official Registration) |
| 🟡 Yellow | Reference to another object (Record) |
| 🟣 Violet | Enums / states |
| 🔵 Light blue | Data fields |

---

## 🔗 Source code

| Class | Repository |
|-------|------------|
| DN00AdministrativeOfficialRegistryRecord | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegistryModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegistryRecord.java) |
| DN00AdministrativeOfficialRegistryRecordState | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegistryModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegistryRecordState.java) |
| DN00AdministrativeOfficialRegistryRecordStateCode | [View code]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegistryModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegistryRecordStateCode.java) |

---

## 🧪 Tests and examples

| Test | Repository |
|------|------------|
| DN00AdminFileTest | [View test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/registry/DN99DENATestMockObjFactoryForAdministrativeOfficialRegistryRecord.java) |
| DN00AdminFileStateTest | [View test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/registry/DN99DENATestMockObjFactoryForAdministrativeOfficialRegistryRecordState.java) |

---

## JSON attributes

| Field | Type | Mandatory | Example | Description |
|-------|------|:---------:|---------|-------------|
| `type` | `String` | ✅ | `"administrativeOfficialRegisterRecord"` | Polymorphic discriminator |
| `oid` | `String` | ✅ | `"REG-OID-001"` | Unique technical identifier |
| `id` | `String` | ✅ | `"REG-2024-00789"` | Business identifier |
| `procedureRecord` | `Object` | ✅ | `{"oid":"EXP-OID-001","id":"EXP-2024-00123"}` *(see [`DN00AdmistrativeServiceProcedureRecord`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java))* | Reference to the record |
| `registeredAt` | `String` (ISO 8601) | ✅ | `"2024-04-10T08:30:00Z"` | Registration date/time |
| `subjectByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Solicitud licencia"}` | Subject of the registry entry |
| `state` | `Object` | ✅ | *(see [`DN00AdministrativeOfficialRegistryRecordState`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegistryModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegistryRecordState.java))* | Current state |
| `state.stateCode` | `String` | ✅ | `"PRESENTED"` | State code |
| `state.description` | `LanguageTexts` | ❌ | `{"SPANISH":"Presentado"}` | Multilingual description |
| `urls` | `Array` | ❌ (recommended) | *(see [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | Access URLs |

---

## States (`state.stateCode`)

| Code | Description |
|------|-------------|
| `PRESENTED` | Presented (direct entry) |
| `RECEIVED_FROM_OTHER_ORG_UNIT` | Received from another unit |
| `TRANSFERRED_FROM_OTHER_ORG_UNIT` | Transferred from another unit |

---

## JSON example

```json
{
  "type": "administrativeOfficialRegisterRecord",
  "oid": "REG-OID-001",
  "id": "REG-2024-00789",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "registeredAt": "2024-04-10T08:30:00Z",
  "subjectByLanguage": {
    "SPANISH": "Solicitud de licencia de obras",
    "BASQUE": "Obra-lizentzia eskaera"
  },
  "state": {
    "stateCode": "PRESENTED",
    "description": { "SPANISH": "Presentado", "BASQUE": "Aurkeztua" }
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/registro/REG-2024-00789", "language": "SPANISH", "tags": ["default"] }
  ]
}
```

---

## Validation rules

- `registeredAt` is mandatory and must be a valid ISO 8601 date.
- `subjectByLanguage` must include at least texts in `SPANISH` and `BASQUE`.
- `state.stateCode` must be one of the values defined in the states table.
- See [validaciones.md](../validaciones.md) for full rules.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
