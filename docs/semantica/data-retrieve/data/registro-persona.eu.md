# :material-account-card: Pertsona Erregistroa (Administrative Person Register)

> - **Bertsioa:** `v{{ dena.version }}`
> - **Data:** {{ dena.date }}
> - **Mota ID (DataTypeID):** `ADMINISTRATIVE_PERSON_REGISTER` · `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` · `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` · `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV`
> - **Testa:** [DN99DENATestMockObjFactoryForAdministrativePersonRegister.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegister.java) ⚠️ EGIAZTATU GABE
> - **Kodea:**
>   - [DN00AdministrativePersonRegisterEntryBase.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/DN00AdministrativePersonRegisterEntryBase.java)
>   - [DN00AdministrativePersonRegisterForRevisionPEV.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForRevisionPEV.java)

## Deskribapena

Pertsona batek administrazio erregistro batean duen inskripzioa adierazten du. `id` eremua erregistro-zenbakia da. Lau aldaera:

- **Oinarrizko erregistroa** (`administrativePersonRegister`) — Erregistro sarrera generikoa
- **PEV Etxebizitza Erregistroa** (`administrativePersonRegisterForHousingPEV`) — Etxebizitza Prestazio Ekonomikoari lotutako erregistroa
- **Azken Borondateen Erregistroa** (`administrativePersonRegisterForLastWill`) — Azken borondateen erregistroa
- **PEV Berrikuspena** (`administrativePersonRegisterForRevisionPEV`) — Hurrengo PEV berrikuspena berrikuste-leiho batekin

> Ikusi baita ere: [campos-comunes.md](./campos-comunes.md) heredatutako eremuetarako (`oid`, `id`, `lastChangedAt`, `urls`, `originAdmin`, `aboutPerson`)

---

## 🔗 Iturburu kodea

| Klasea | Biltegia |
|--------|----------|
| DN00AdministrativePersonRegisterEntryBase | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/DN00AdministrativePersonRegisterEntryBase.java) |
| DN00AdministrativePersonRegisterForHousingPEV | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForHousingPEV.java) |
| DN00AdministrativePersonRegisterForLastWill | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForLastWill.java) |
| DN00AdministrativePersonRegisterForRevisionPEV | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativePersonRegisterModelClasses/src/main/java/dena/api/data/model/personregister/datatypes/DN00AdministrativePersonRegisterForRevisionPEV.java) |

---

## 🧪 Testak eta adibideak

| Testa | Biltegia |
|-------|----------|
| DN99DENATestMockObjFactoryForAdministrativePersonRegister | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/personregister/DN99DENATestMockObjFactoryForAdministrativePersonRegister.java) ⚠️ EGIAZTATU GABE |

---

## Eremu komunak

<span class="field-badge--common">komun</span> markarekin izendatutako eremuak [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java)-tik **heredatutakoak** dira. Ikusi [campos-comunes.md](./campos-comunes.md).

| Eremua | Mota | Derrigorrezkoa | Adibidea | Deskribapena |
|--------|------|:--------------:|----------|--------------|
| `type` | `String` | ✅ | `"administrativePersonRegister"` | Mota diskriminatzailea |
| `oid` <span class="field-badge--common">komun</span> | `String` | ✅ | `"REG-OID-001"` | Identifikatzaile tekniko bakarra |
| `id` <span class="field-badge--common">komun</span> | `String` | ✅ | `"REG-2024-00001"` | Erregistro-zenbakia |
| `lastChangedAt` <span class="field-badge--common">komun</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Jatorrizko datuen azken aldaketa |
| `registeredAt` | `Instant` | ✅ | `"2023-09-01T00:00:00Z"` | Inskripzio data |
| `expiresAt` | `Instant` | ❌ | `"2025-09-01T00:00:00Z"` | Iraungite data (null mugagabea bada) |
| `state` | `Object` | ✅ | `{"stateCode":"ACTIVE"}` | Inskripzioaren egoera |

---

## PEV Berrikuspena — Eremu gehigarriak

| Eremua | Mota | Derrigorrezkoa | Adibidea | Deskribapena |
|--------|------|:--------------:|----------|--------------|
| `startDate` | `Instant` | ✅ | `"2025-01-01T00:00:00Z"` | Berrikuste-leihoaren hasiera |
| `endDate` | `Instant` | ✅ | `"2025-03-31T00:00:00Z"` | Berrikuste-leihoaren amaiera |

---

## Erregistroaren egoerak (`state.stateCode`)

| Kodea | Deskribapena |
|-------|--------------|
| `REGISTERED` | Erregistratuta (aktibatzeko zain) |
| `ACTIVE` | Inskripzio aktiboa eta indarrean |
| `EXPIRED` | Inskripzio iraungitua |
| `CANCELLED` | Inskripzio baliogabetua |

---

## Adibidea — PEV Etxebizitza Erregistroa

<div class="common-highlight" markdown>

```json hl_lines="3 4 5"
{
  "type": "administrativePersonRegisterForHousingPEV",
  "oid": "REG-OID-001",
  "id": "REG-PEV-2024-00001",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "registeredAt": "2023-09-01T00:00:00Z",
  "expiresAt": "2025-09-01T00:00:00Z",
  "state": { "stateCode": "ACTIVE" }
}
```

<p class="common-fields-legend"><span class="swatch"></span> Nabarmendutako lerroak oinarrizko klasetik <strong>heredatutako eremu komunak</strong> dira (ikusi <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
