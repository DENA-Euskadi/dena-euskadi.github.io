# :material-school: Matrikula (Person Enrollment)

> - **Bertsioa:** `v{{ dena.version }}`
> - **Data:** {{ dena.date }}
> - **Mota ID (DataTypeID):** `PERSON_ENROLLMENT` · `PERSON_ACADEMIC_ENROLLMENT`
> - **Testa:** [DN99DENATestMockObjFactoryForPersonEnrollment.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/enrollment/DN99DENATestMockObjFactoryForPersonEnrollment.java)
> - **Kodea:**
>   - [DN00PersonEnrollmentBase.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/DN00PersonEnrollmentBase.java)
>   - [DN00PersonAcademicEnrollment.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/academic/DN00PersonAcademicEnrollment.java)

## Deskribapena

Pertsona batek ikastaro, programa edo jarduera batean duen matrikula adierazten du. Bi aldaera:

- **Oinarrizko matrikula** (`personEnrollment`) — Edozein jarduera edo programatarako matrikula orokorra
- **Matrikula akademikoa** (`personAcademicEnrollment`) — Ikasturtearekin eta hezkuntza mailarekin espezializazioa

> Ikusi baita ere: [campos-comunes.md](./campos-comunes.md) heredatutako eremuetarako (`oid`, `id`, `lastChangedAt`, `urls`, `originAdmin`, `aboutPerson`)

---

## 🔗 Iturburu kodea

| Klasea | Biltegia |
|--------|----------|
| DN00PersonEnrollmentBase | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/DN00PersonEnrollmentBase.java) |
| DN00PersonAcademicEnrollment | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/academic/DN00PersonAcademicEnrollment.java) |
| DN00PersonEnrollmentState | [Ikusi kodea]({{ repos.common_data_api_blob }}/denaCommonDataAPIPersonEnrollmentModelClasses/src/main/java/dena/api/data/model/enrollment/DN00PersonEnrollmentState.java) |

---

## 🧪 Testak eta adibideak

| Testa | Biltegia |
|-------|----------|
| DN99DENATestMockObjFactoryForPersonEnrollment | [Ikusi testa]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/enrollment/DN99DENATestMockObjFactoryForPersonEnrollment.java) |

---

## Eremu komunak

<span class="field-badge--common">komun</span> markarekin izendatutako eremuak [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java)-tik **heredatutakoak** dira. Ikusi [campos-comunes.md](./campos-comunes.md).

| Eremua | Mota | Derrigorrezkoa | Adibidea | Deskribapena |
|--------|------|:--------------:|----------|--------------|
| `type` | `String` | ✅ | `"personEnrollment"` | Mota diskriminatzailea |
| `oid` <span class="field-badge--common">komun</span> | `String` | ✅ | `"ENR-OID-001"` | Identifikatzaile tekniko bakarra |
| `id` <span class="field-badge--common">komun</span> | `String` | ✅ | `"MAT-2024-00001"` | Negozio identifikatzailea (matrikula-zenbakia) |
| `lastChangedAt` <span class="field-badge--common">komun</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Jatorrizko datuen azken aldaketa |
| `registeredAt` | `Instant` | ✅ | `"2024-09-01T00:00:00Z"` | Matrikula erregistratu zen data |
| `enrollmentDateRange` | `Object` | ✅ | `{"start":"2024-09-01T00:00:00","end":"2025-06-30T00:00:00"}` | Matrikula-aldiaren data tartea |
| `subjectByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Inglés B2 — Módulo 3"}` | Ikastaroaren edo matrikularen izena hainbat hizkuntzatan |
| `institutionNameByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"IES Zurbaran","BASQUE":"IES Zurbaran"}` | Ikastetxearen izena hainbat hizkuntzatan |
| `state` | `Object` | ✅ | `{"stateCode":"ENROLLED"}` | Matrikularen egoera |

---

## Matrikula akademikoa — Eremu gehigarriak

| Eremua | Mota | Derrigorrezkoa | Adibidea | Deskribapena |
|--------|------|:--------------:|----------|--------------|
| `academicYear` | `Year` | ✅ | `2024` | Ikasturtearen hasiera urtea (adib. `2024` 2024-2025 ikasturterako) |
| `levelByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Bachillerato","BASQUE":"Batxilergoa"}` | Hezkuntza maila hainbat hizkuntzatan |

---

## Matrikularen egoerak (`state.stateCode`)

| Kodea | Deskribapena |
|-------|--------------|
| `ENROLLED` | Matrikula aktiboa |
| `CANCELLED` | Baliogabetutako matrikula |

---

## Adibidea — Oinarrizko matrikula

<div class="common-highlight" markdown>

```json hl_lines="3 4 5"
{
  "type": "personEnrollment",
  "oid": "ENR-OID-001",
  "id": "MAT-2024-00001",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "registeredAt": "2024-09-01T00:00:00Z",
  "enrollmentDateRange": {
    "start": "2024-09-01T00:00:00",
    "end": "2025-06-30T00:00:00"
  },
  "subjectByLanguage": { "SPANISH": "Inglés B2 — Módulo 3", "BASQUE": "Ingelesa B2 — 3. modulua" },
  "institutionNameByLanguage": { "SPANISH": "Escuela Oficial de Idiomas Bilbao", "BASQUE": "Bilboko Hizkuntza Eskola Ofiziala" },
  "state": { "stateCode": "ENROLLED" }
}
```

<p class="common-fields-legend"><span class="swatch"></span> Nabarmendutako lerroak oinarrizko klasetik <strong>heredatutako eremu komunak</strong> dira (ikusi <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
