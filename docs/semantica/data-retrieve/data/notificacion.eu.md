# :material-email-open: Jakinarazpena (Administrative Notice)

> - **Bertsioa:** `v{{ dena.version }}`
> - **Data:** {{ dena.date }}
> - **Mota ID (DataTypeID):** `ADMINISTRATIVE_NOTICE`
> - **Testa:** [DN99DENATestMockObjFactoryForAdministrativeNotice.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java)
> - **Kodea:** [DN00AdministrativeNotice.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNotice.java)

## Deskribapena

Herritarrei zuzendutako jakinarazpen ofizial edo administrazio-komunikazio bat adierazten du, espediente bati lotuta.

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
    NOT["<b>Jakinarazpena</b><br/><i>administrativeNotice</i>"]

    NOT --> EXP["record<br/><i>oid · id</i>"]
    NOT --> TYPE["typeNotice<br/><i>OFFICIAL_NOTICE<br/>COMMUNICATION</i>"]
    NOT --> DATES["issuedAt<br/>readAt"]
    NOT --> STATE["state<br/><i>(string zuzena)</i>"]
    NOT --> SUBJECT["subjectByLanguage"]
    NOT --> URLS["urls[]"]

    style NOT fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style EXP fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style TYPE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style DATES fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style STATE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style SUBJECT fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style URLS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click EXP "../expediente/" "Ikusi Espedientea"
```

| Kolorea | Esanahia |
|---------|----------|
| 🟠 Laranja | Objektu nagusia (Jakinarazpena) |
| 🟡 Horia | Beste objektu baten erreferentzia (Espedientea) |
| 🟣 Morea | Enumak / egoerak / motak |
| 🔵 Urdin argia | Datu-eremuak |

---

## 🔗 Iturburu-kodea

| Klasea | Biltegia |
|--------|----------|
| DN00AdministrativeNotice | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNotice.java) |
| DN00AdministrativeNoticeStateCode | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNoticeStateCode.java) |
| DN00AdministrativeNoticeType | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNoticeType.java) |

---

## 🧪 Testak eta adibideak

| Testa | Biltegia |
|-------|----------|
| DN00NotificationTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java) |
| DN00NotificationStatusTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNoticeStateCode.java) |
| DN00NotificationTypeTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNoticeType.java) |
| DN00NotificationIDTest | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java) |

---

## JSON atributuak

<span class="field-badge--common">komuna</span> markarekin adierazitako eremuak [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java) oinarri-klasetik **heredatutakoak** dira eta data-retrieve mota guztietan daude. Ikusi [campos-comunes.md](./campos-comunes.md).

| Eremua | Mota | Derrigorrez | Adibidea | Deskribapena |
|--------|------|:-----------:|----------|--------------|
| `type` | `String` | ✅ | `"administrativeNotice"` | Diskriminatzaile polimorfiko (beti `"administrativeNotice"`) |
| `typeNotice` | `String` (enum) | ✅ | `"OFFICIAL_NOTICE"` | Jakinarazpen mota: `OFFICIAL_NOTICE` edo `COMMUNICATION` (`DN00AdministrativeNoticeType` enum-a). Ikusi oharra behean. |
| `oid` <span class="field-badge--common">komuna</span> | `String` | ✅ | `"NOT-OID-001"` | Identifikatzaile tekniko bakarra |
| `id` <span class="field-badge--common">komuna</span> | `String` | ✅ | `"NOT-2024-00456"` | Negozio-identifikatzailea |
| `lastChangedAt` <span class="field-badge--common">komuna</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Datuaren azken aldaketa jatorrian. DENA-COREk erabiltzen du UIan NEW/UPDATED/UNCHANGED egoera kalkulatzeko |
| `record` | `Object` | ✅ | `{"oid":"EXP-OID-001","id":"EXP-2024-00123"}` *(ikusi [`DN00AdmistrativeServiceProcedureRecord`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java))* | Espedientearen erreferentzia |
| `issuedAt` | `String` (ISO 8601) | ✅ | `"2024-05-20T09:00:00Z"` | Igorpen-data |
| `readAt` | `String` (ISO 8601) | ❌ | `"2024-05-21T10:30:00Z"` | Irakurketa-data (null irakurri ez bada) |
| `state` | `String` | ✅ | `"PENDING_TO_BE_READ_BY_DESTINATION"` | Uneko egoera (string zuzena, EZ objektua) |
| `subjectByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Resolución de ayuda"}` | Jakinarazitako egintzaren gaia |
| `urls` <span class="field-badge--common">komuna</span> | `Array` | ❌ (gomendatua) | *(ikusi [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | Sarbide-URLak |

> **Garrantzitsua:** Jakinarazpenetan, `state` eremua zuzenean **string** bat da egoera-kodearekin (adib. `"PENDING_TO_BE_READ_BY_DESTINATION"`), espediente eta erregistroetan ez bezala, non `state` `stateCode` eta `description`-rekin objektu bat den.

---

## Motak (`typeNotice`)

| Kodea | Deskribapena |
|-------|--------------|
| `OFFICIAL_NOTICE` | Jakinarazpen administratibo ofiziala |
| `COMMUNICATION` | Administrazio-komunikazioa |

> **Oharra:** `typeNotice` eremuak jakinarazpena sailkatzen du (`OFFICIAL_NOTICE` / `COMMUNICATION`). Ez nahastu `type` diskriminatzaile polimorfikoarekin, zeina beti `"administrativeNotice"` balioa duen eta serializatzaileak automatikoki kudeatzen duen.

---

## Egoerak (`state`)

| Kodea | Deskribapena |
|-------|--------------|
| `PENDING_TO_BE_READ_BY_DESTINATION` | Irakurtzeko zain |
| `ACKNOWLEDGED_BY_DESTINATION` | Irakurrita eta onartuta |
| `REJECTED_BY_DESTINATION` | Ukatuta |
| `EXPIRED` | Iraungita |
| `CANCELLED_BY_ISSUER` | Igorlearen aldetik bertan behera utzita |
| `DELETED_BY_ISSUER` | Igorlearen aldetik ezabatuta |

---

## JSON adibidea

<div class="common-highlight" markdown>

```json hl_lines="3 4 5 6"
{
  "type": "administrativeNotice",
  "oid": "NOT-OID-001",
  "id": "NOT-2024-00456",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "urls": [
    { "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00456", "language": "SPANISH", "tags": ["default"] }
  ],
  "typeNotice": "OFFICIAL_NOTICE",
  "record": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "issuedAt": "2024-05-20T09:00:00Z",
  "readAt": null,
  "state": "PENDING_TO_BE_READ_BY_DESTINATION",
  "subjectByLanguage": {
    "SPANISH": "Resolución de concesión de ayuda",
    "BASQUE": "Laguntza emateko ebazpena"
  }
}
```

<p class="common-fields-legend"><span class="swatch"></span> Nabarmendutako lerroak oinarri-klasetik <strong>heredatutako</strong> eremu <strong>komunak</strong> dira (ikusi <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

### Adibidea — Irakurritako jakinarazpena

<div class="common-highlight" markdown>

```json hl_lines="3 4 5 6"
{
  "type": "administrativeNotice",
  "oid": "NOT-OID-002",
  "id": "NOT-2024-00457",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "urls": [
    { "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00457", "language": "SPANISH", "tags": ["default"] }
  ],
  "typeNotice": "COMMUNICATION",
  "record": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "issuedAt": "2024-05-15T08:00:00Z",
  "readAt": "2024-05-16T10:30:00Z",
  "state": "ACKNOWLEDGED_BY_DESTINATION",
  "subjectByLanguage": {
    "SPANISH": "Comunicación de subsanación de documentación",
    "BASQUE": "Dokumentazioa zuzentzeko jakinarazpena"
  }
}
```

<p class="common-fields-legend"><span class="swatch"></span> Nabarmendutako lerroak oinarri-klasetik <strong>heredatutako</strong> eremu <strong>komunak</strong> dira (ikusi <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

## Balioztatze-arauak

- `state` = `PENDING_TO_BE_READ_BY_DESTINATION` bada, `readAt` `null` izan behar da.
- `state` = `ACKNOWLEDGED_BY_DESTINATION` edo `REJECTED_BY_DESTINATION` bada, `readAt`-ek balio bat izan behar du.
- `issuedAt` `readAt` baino lehenagokoa edo berdina izan behar da (existitzen bada).
- Ikusi [validaciones.md](../validaciones.md) arau osoentzat.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
