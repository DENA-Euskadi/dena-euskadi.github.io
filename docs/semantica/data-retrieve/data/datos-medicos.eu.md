# :material-hospital-box: Datu Medikoak (Person Medical Data)

> - **Bertsioa:** `v{{ dena.version }}`
> - **Data:** {{ dena.date }}
> - **Mota ID (DataTypeID):** `PERSON_MEDICAL_DATA`
> - **Testa:** [DN99DENATestMockObjFactoryForPersonMedicalData.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/persondata/medical/DN99DENATestMockObjFactoryForPersonMedicalData.java) ⚠️ EGIAZTATU GABE
> - **Kodea:** [DN00PersonMedicalData.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonDataModelClasses/src/main/java/dena/api/data/model/persondata/medical/DN00PersonMedicalData.java)

## Deskribapena

Pertsona baten datu medikoak adierazten ditu: osasun identifikatzaileak, esleitutako medikua eta osasun-kontaktu datuak.

!!! warning "Identifikatu GABEKO objektua — oid eta id gabe"
    `DN00PersonMedicalData`-k (`DN00PersonData` bidez) `DN00DENADataExchangedNOTIdentifiedObject` hedatzen du. Objektu honek **EZ ditu** `oid` eta `id`, baina **BAI ditu** `lastChangedAt`, `urls`, `originAdmin` eta `aboutPerson`.

---

## 🔗 Iturburu kodea

| Klasea | Biltegia |
|--------|----------|
| DN00PersonMedicalData | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonDataModelClasses/src/main/java/dena/api/data/model/persondata/medical/DN00PersonMedicalData.java) |
| DN00PersonData | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonDataModelClasses/src/main/java/dena/api/data/model/persondata/DN00PersonData.java) |
| DN00DENADataExchangedNOTIdentifiedObject | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedNOTIdentifiedObject.java) |

---

## 🧪 Testak eta adibideak

| Testa | Biltegia |
|-------|----------|
| DN99DENATestMockObjFactoryForPersonMedicalData | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/persondata/medical/DN99DENATestMockObjFactoryForPersonMedicalData.java) ⚠️ EGIAZTATU GABE |

---

## Eremuak

Data-retrieve-ko beste mota batzuek ez bezala, **objektu honek ez ditu `oid` eta `id` eremu komunak** oinarrizko klase estandarretik heredatzen, `DN00DENADataExchangedNOTIdentifiedObject` hedatzen duelako. Datu medikoak identifikatzaile propiorik gabeko datu pertsonalak dira, baina `lastChangedAt` bai dute.

| Eremua | Mota | Derrigorrezkoa | Adibidea | Deskribapena |
|--------|------|:--------------:|----------|--------------|
| `type` | `String` | ✅ | `"personMedicalData"` | Mota diskriminatzailea (beti `"personMedicalData"`) |
| `lastChangedAt` | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Datuaren azken aldaketa jatorrian |
| `medicalPassId` | `String` | ❌ | `"TIS-123456789"` | Osasun Txartel Indibiduala (TIS) — txartel-zenbakia |
| `medicalInsuranceId` | `String` | ❌ | `"MUFACE-00123456"` | Osagarrizko osasun aseguruaren identifikatzailea |
| `doctorFullName` | `PersonFullName` | ❌ | `{"name":"Juan","surname1":"García","surname2":"López"}` | Esleitutako medikuaren izen osoa |
| `medicalCenterGeoPosition` | `Object` | ❌ | `{"latitude":43.2627,"longitude":-2.9253}` | Osasun-zentroko geolokalizazioa |
| `contactData` | `Object` | ❌ | *(DN00PersonData-tik heredatua)* | Kontaktu datuak |
| `bankAccountData` | `Object` | ❌ | *(DN00PersonData-tik heredatua)* | Banku kontuaren datuak |

---

## Adibidea

```json
{
  "type": "personMedicalData",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "medicalPassId": "TIS-123456789",
  "medicalInsuranceId": null,
  "doctorFullName": {
    "name": "María",
    "surname1": "Fernández",
    "surname2": "Ruiz"
  },
  "medicalCenterGeoPosition": {
    "latitude": 43.2627,
    "longitude": -2.9253
  }
}
```

!!! info "Mota honetan ez dago oid eta id"
    Beste datu motak ez bezala, objektu honek ez du `oid` eta `id` eremurik. Data-retrieve eskabidearen testuinguruko pertsonari zuzenean lotua dago, baina `lastChangedAt` bai du.

---

## Oharrak

- Datu medikoak **berezi babestutako datuak** dira (DBEO 9. Art.). Baimen egokiarekin soilik transferitu behar dira.
- `medicalPassId` Osasun Txartel Indibiduala (TIS) edo baliokidea da (TSE, Europako Osasun Txartel Indibiduala) kasuen arabera.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
