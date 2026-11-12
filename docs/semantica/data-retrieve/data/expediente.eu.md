# :material-folder-open: Espedientea (Procedure Record)

> - **Bertsioa:** `v{{ dena.version }}`
> - **Data:** {{ dena.date }}
> - **Mota ID (DataTypeID):** `ADMINISTRATIVE_RECORD`
> - **Testa:** [DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java)
> - **Kodea:** [DN00AdmistrativeServiceProcedureRecord.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java)

## Deskribapena

Zerbitzu eta prozedura bati lotutako administrazio-espediente bat adierazten du. Ereduaren objektu nagusia da: jakinarazpenak, erregistroak eta ordainketak haren mende daude.

> Ikusi baita ere: [campos-comunes.md](./campos-comunes.md) heredatutako eremuetarako (`oid`, `id`, `lastChangedAt`, `urls`, `originAdmin`, `aboutPerson`)

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
    EXP["<b>Espedientea</b><br/><i>administrativeServiceProcedureRecord</i>"]

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

    click SRV "../servicio-administrativo/" "Ikusi Administrazio-zerbitzua"
    click PROC "../servicio-administrativo/" "Ikusi Prozedura"
```

| Kolorea | Esanahia |
|---------|----------|
| 🟠 Laranja | Objektu nagusia (Espedientea) |
| ⚪ Grisa | Testuinguru hierarkikoa (Zerbitzua / Prozedura) |
| 🟣 Morea | Enumak / egoerak |
| 🔵 Urdin argia | Datu-eremuak |

---

## 🔗 Iturburu-kodea

| Klasea | Biltegia |
|--------|----------|
| DN00AdmistrativeServiceProcedureRecord | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java) |
| DN00AdmistrativeServiceProcedureApplicationData | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureApplicationData.java) |
| DN00AdmistrativeServiceProcedureRecordData | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecordData.java) |
| DN00AdministrativeServiceInterestedParty | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceInterestedParty.java) |
| DN00AdministrativeServiceProcedureRecordState | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedureRecordState.java) |
| DN00AdministrativeServiceProcedureRecordStateCode | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedureRecordStateCode.java) |

---

## 🧪 Testak eta adibideak

| Testa | Biltegia |
|-------|----------|
| DN00RecordTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |
| DN00RecordStatusTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdministrativeServiceProcedureRecordState.java) |
| DN00RecordIDTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |

---

## JSON atributuak

<span class="field-badge--common">komuna</span> markarekin adierazitako eremuak [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java) oinarri-klasetik **heredatutakoak** dira eta data-retrieve mota guztietan daude. Ikusi [campos-comunes.md](./campos-comunes.md).

| Eremua | Mota | Derrigorrez | Adibidea | Deskribapena |
|--------|------|:-----------:|----------|--------------|
| `type` | `String` | ✅ | `"administrativeServiceProcedureRecord"` | Diskriminatzaile polimorfiko |
| `oid` <span class="field-badge--common">komuna</span> | `String` | ✅ | `"EXP-OID-001"` | Identifikatzaile tekniko bakarra |
| `id` <span class="field-badge--common">komuna</span> | `String` | ✅ | `"EXP-2024-00123"` | Negozio-identifikatzailea |
| `lastChangedAt` <span class="field-badge--common">komuna</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Datuaren azken aldaketa jatorrian. DENA-COREk erabiltzen du UIan NEW/UPDATED/UNCHANGED egoera kalkulatzeko |
| `service` | `Object` | ✅ | *(ikusi [servicio-administrativo.md](./servicio-administrativo.md) · [`DN00AdmistrativeService`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeService.java))* | Administrazio-zerbitzua |
| `procedure` | `Object` | ✅ | *(ikusi [servicio-administrativo.md](./servicio-administrativo.md) · [`DN00AdministrativeServiceProcedure`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedure.java))* | Prozedura |
| `applicationData` | `Object` | ❌ | *(ikusi [applicationData](#applicationdata))* | Eskaera aurkeztearen datuak |
| `applicationData.date` | `String` (ISO 8601) | ❌ | `"2024-03-14T09:00:00Z"` | Eskaera aurkezteko data |
| `applicationData.ledgerId` | `String` | ❌ | `"REG-2024-00123"` | Esleitutako erregistro-idazpenaren zenbakia |
| `recordData` | `Object` | ❌ | *(ikusi [recordData](#recorddata))* | Espedientearen datuak (egoera, datak, interesduna) |
| `recordData.createdAt` | `String` (ISO 8601) | ❌ | `"2024-03-15T10:30:00Z"` | Espedientearen sorrera-data |
| `recordData.lastModifiedAt` | `String` (ISO 8601) | ❌ | `"2024-06-01T14:00:00Z"` | Azken aldaketaren data |
| `recordData.state` | `Object` | ❌ | `{"stateCode":"IN_PROGRESS"}` | Espedientearen egoera |
| `recordData.state.stateCode` | `String` | ❌ | `"IN_PROGRESS"` | Egoera-kodea |
| `recordData.state.description` | `LanguageTexts` | ❌ | `{"SPANISH":"En tramitación"}` | Egoeraren deskribapen eleanitza |
| `recordData.interestedParty` | `Object` | ❌ | `{"personId":"12345678A","displayName":"Juan García"}` | Espedientean interesduna |
| `recordData.interestedParty.personId` | `String` | ❌ | `"12345678A"` | Interesdunaren IFZ/NAN |
| `recordData.interestedParty.displayName` | `String` | ❌ | `"Juan García"` | Interesdunaren izena |
| `descriptionByLanguage` | `LanguageTexts` | ❌ | `{"SPANISH":"Licencia apertura"}` | Espedientearen deskribapena |
| `urls` <span class="field-badge--common">komuna</span> | `Array` | ❌ (gomendatua) | *(ikusi [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | Egoitza elektronikorako sarbide-URLak |

---

## Egoerak (`state.stateCode`)

| Kodea | Deskribapena |
|-------|--------------|
| `REGISTERED_PENDING_TO_BE_OPENED` | Erregistratua, irekitzeko zain |
| `OPENED` | Irekita |
| `IN_PROGRESS` | Izapidetzen |
| `WAITING_FOR_INTERESTED_PARTY_RESPONSE` | Interesdunaren erantzunaren zain |
| `WAITING_FOR_OTHER_ORG_WORK` | Beste erakunde baten zain |
| `CLOSED` | Itxita |

---

## JSON adibidea

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

<p class="common-fields-legend"><span class="swatch"></span> Nabarmendutako lerroak oinarri-klasetik <strong>heredatutako</strong> eremu <strong>komunak</strong> dira (ikusi <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

## Beste objektuekin harremana

Ondorengo objektuek espedienteari erreferentzia egiten diote `record` bidez (lehen `procedureRecord`):

- Jakinarazpena (`administrativeNotice`)
- Erregistro Liburua (`administrativeLedger`)
- Ordainketa (`oneOffPayment`, `directDebitPayment`)
- Onura ekonomikoa (`economicBenefit`)
- Pertsonaren kredentziala (`personCredential`)
- Hitzordua (`scheduleItem`) — `record` eremua aukerakoa da hitzorduetan

---

## Balioztatze-arauak

- `service` eta `procedure`-k gutxienez `nameByLanguage` sartu behar dute `SPANISH` eta `BASQUE` testuekin.
- `recordData.createdAt`, sartzen bada, ISO 8601 data balioduna izan behar du.
- `recordData.lastModifiedAt` sartzen bada, `recordData.createdAt` ≥ izan behar du.
- Ikusi [validaciones.md](../validaciones.md) arau osoentzat.


<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
