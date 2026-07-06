# :material-book-open-page-variant: Registro Oficial (Official Register Record)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **Test:** [DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/register/DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java)
> - **Código:** [DN00AdministrativeOfficialRegisterRecord.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegisterModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegisterRecord.java)

## Descripción

Representa un asiento registral de entrada o salida en un registro oficial, vinculado a un expediente.

> Ver también: [campos-comunes.md](./campos-comunes.md) para campos heredados (`oid`, `id`, `urls`, `originAdminRef`, `aboutPersonRef`)

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
    REG["<b>Registro Oficial</b><br/><i>administrativeOfficialRegisterRecord</i>"]

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

    click EXP "../expediente/" "Ver Expediente"
```

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (Registro Oficial) |
| 🟡 Amarillo | Referencia a otro objeto (Expediente) |
| 🟣 Violeta | Enums / estados |
| 🔵 Azul claro | Campos de datos |

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00AdministrativeOfficialRegisterRecord | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegisterModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegisterRecord.java) |
| DN00AdministrativeOfficialRegisterRecordState | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegisterModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegisterRecordState.java) |
| DN00AdministrativeOfficialRegisterRecordStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegisterModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegisterRecordStateCode.java) |

---

## 🧪 Tests y ejemplos

| Test | Repositorio |
|------|-------------|
| DN00AdminFileTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/register/DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java) |
| DN00AdminFileStateTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/register/DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecordState.java) |

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"administrativeOfficialRegisterRecord"` | Discriminador polimórfico |
| `oid` | `String` | ✅ | `"REG-OID-001"` | Identificador técnico único |
| `id` | `String` | ✅ | `"REG-2024-00789"` | Identificador de negocio |
| `procedureRecord` | `Object` | ✅ | `{"oid":"EXP-OID-001","id":"EXP-2024-00123"}` *(ver [`DN00AdmistrativeServiceProcedureRecord`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java))* | Referencia al expediente |
| `registeredAt` | `String` (ISO 8601) | ✅ | `"2024-04-10T08:30:00Z"` | Fecha/hora de registro |
| `subjectByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Solicitud licencia"}` | Asunto del asiento registral |
| `state` | `Object` | ✅ | *(ver [`DN00AdministrativeOfficialRegisterRecordState`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeOfficialRegisterModelClasses/src/main/java/dena/api/data/model/register/DN00AdministrativeOfficialRegisterRecordState.java))* | Estado actual |
| `state.stateCode` | `String` | ✅ | `"PRESENTED"` | Código de estado |
| `state.description` | `LanguageTexts` | ❌ | `{"SPANISH":"Presentado"}` | Descripción multiidioma |
| `urls` | `Array` | ❌ (recomendado) | *(ver [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | URLs de acceso |

---

## Estados (`state.stateCode`)

| Código | Descripción |
|--------|-------------|
| `PRESENTED` | Presentado (entrada directa) |
| `RECEIVED_FROM_OTHER_ORG_UNIT` | Recibido desde otra unidad |
| `TRANSFERRED_FROM_OTHER_ORG_UNIT` | Transferido desde otra unidad |

---

## Ejemplo JSON

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

## Reglas de validación

- `registeredAt` es obligatorio y debe ser una fecha ISO 8601 válida.
- `subjectByLanguage` debe incluir al menos textos en `SPANISH` y `BASQUE`.
- `state.stateCode` debe ser uno de los valores definidos en la tabla de estados.
- Ver [validaciones.md](../validaciones.md) para reglas completas.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
