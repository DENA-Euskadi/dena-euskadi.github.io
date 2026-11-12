# :material-email-open: Notificación (Administrative Notice)

> - **Versión:** `v{{ dena.version }}`
> - **Fecha:** {{ dena.date }}
> - **ID de Tipo (DataTypeID):** `ADMINISTRATIVE_NOTICE`
> - **Test:** [DN99DENATestMockObjFactoryForAdministrativeNotice.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java)
> - **Código:** [DN00AdministrativeNotice.java]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNotice.java)

## Descripción

Representa una notificación oficial o comunicación administrativa emitida hacia una persona ciudadana, vinculada a un expediente.

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
    NOT["<b>Notificación</b><br/><i>administrativeNotice</i>"]

    NOT --> EXP["record<br/><i>oid · id</i>"]
    NOT --> TYPE["typeNotice<br/><i>OFFICIAL_NOTICE<br/>COMMUNICATION</i>"]
    NOT --> DATES["issuedAt<br/>readAt"]
    NOT --> STATE["state<br/><i>(string directo)</i>"]
    NOT --> SUBJECT["subjectByLanguage"]
    NOT --> URLS["urls[]"]

    style NOT fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style EXP fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style TYPE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style DATES fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style STATE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style SUBJECT fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style URLS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click EXP "../expediente/" "Ver Expediente"
```

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (Notificación) |
| 🟡 Amarillo | Referencia a otro objeto (Expediente) |
| 🟣 Violeta | Enums / estados / tipos |
| 🔵 Azul claro | Campos de datos |

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00AdministrativeNotice | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNotice.java) |
| DN00AdministrativeNoticeStateCode | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNoticeStateCode.java) |
| DN00AdministrativeNoticeType | [Ver código]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeNoticeModelClasses/src/main/java/dena/api/data/model/administrativenotice/DN00AdministrativeNoticeType.java) |

---

## 🧪 Tests y ejemplos

| Test | Repositorio |
|------|-------------|
| DN00NotificationTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java) |
| DN00NotificationStatusTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNoticeStateCode.java) |
| DN00NotificationTypeTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNoticeType.java) |
| DN00NotificationIDTest | [Ver test]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java) |

---

## Atributos JSON

Los campos marcados con <span class="field-badge--common">común</span> son **heredados** de [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java) y están presentes en todos los tipos del data-retrieve. Ver [campos-comunes.md](./campos-comunes.md).

| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `type` | `String` | ✅ | `"administrativeNotice"` | Discriminador polimórfico (siempre `"administrativeNotice"`) |
| `typeNotice` | `String` (enum) | ✅ | `"OFFICIAL_NOTICE"` | Tipo de notificación: `OFFICIAL_NOTICE` o `COMMUNICATION` (enum `DN00AdministrativeNoticeType`). Ver nota abajo. |
| `oid` <span class="field-badge--common">común</span> | `String` | ✅ | `"NOT-OID-001"` | Identificador técnico único |
| `id` <span class="field-badge--common">común</span> | `String` | ✅ | `"NOT-2024-00456"` | Identificador de negocio |
| `lastChangedAt` <span class="field-badge--common">común</span> | `Instant` | ✅ | `"2026-08-19T08:07:56.742Z"` | Última modificación del dato en el origen. DENA-CORE lo usa para calcular el estado NEW/UPDATED/UNCHANGED |
| `record` | `Object` | ✅ | `{"oid":"EXP-OID-001","id":"EXP-2024-00123"}` *(ver [`DN00AdmistrativeServiceProcedureRecord`]({{ repos.common_data_api_blob }}/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceProcedureRecord.java))* | Referencia al expediente |
| `issuedAt` | `String` (ISO 8601) | ✅ | `"2024-05-20T09:00:00Z"` | Fecha de emisión |
| `readAt` | `String` (ISO 8601) | ❌ | `"2024-05-21T10:30:00Z"` | Fecha de lectura (null si no leída) |
| `state` | `String` | ✅ | `"PENDING_TO_BE_READ_BY_DESTINATION"` | Estado actual (string directo, NO objeto) |
| `subjectByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Resolución de ayuda"}` | Asunto del acto notificado |
| `urls` <span class="field-badge--common">común</span> | `Array` | ❌ (recomendado) | *(ver [campos-comunes.md](./campos-comunes.md) · [`DN00DENADataExchangedObjectBase`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DENADataExchangedObjectBase.java))* | URLs de acceso |

> **Importante:** En notificaciones, el campo `state` es directamente un **string** con el código de estado (ej: `"PENDING_TO_BE_READ_BY_DESTINATION"`), a diferencia de expedientes y registros donde `state` es un objeto con `stateCode` y `description`.

---

## Tipos (`typeNotice`)

| Código | Descripción |
|--------|-------------|
| `OFFICIAL_NOTICE` | Notificación administrativa oficial |
| `COMMUNICATION` | Comunicación administrativa |

> **Nota:** El campo `typeNotice` clasifica la notificación (`OFFICIAL_NOTICE` / `COMMUNICATION`). No confundir con el campo `type` del discriminador polimórfico, que siempre vale `"administrativeNotice"` y lo gestiona el serializador automáticamente.

---

## Estados (`state`)

| Código | Descripción |
|--------|-------------|
| `PENDING_TO_BE_READ_BY_DESTINATION` | Pendiente de lectura |
| `ACKNOWLEDGED_BY_DESTINATION` | Leída y aceptada |
| `REJECTED_BY_DESTINATION` | Rechazada |
| `EXPIRED` | Expirada |
| `CANCELLED_BY_ISSUER` | Cancelada por el emisor |
| `DELETED_BY_ISSUER` | Eliminada por el emisor |

---

## Ejemplo JSON

<div class="common-highlight" markdown>

```json hl_lines="3 4 5 6"
{
  "type": "administrativeNotice",
  "oid": "NOT-OID-001",
  "id": "NOT-2024-00456",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "urls": [{ "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00456", "language": "SPANISH", "tags": ["default"] }],
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

<p class="common-fields-legend"><span class="swatch"></span> Las líneas resaltadas son los campos <strong>comunes</strong> heredados de la clase base (ver <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

### Ejemplo — Notificación leída

<div class="common-highlight" markdown>

```json hl_lines="3 4 5 6"
{
  "type": "administrativeNotice",
  "oid": "NOT-OID-002",
  "id": "NOT-2024-00457",
  "lastChangedAt": "2026-08-19T08:07:56.742Z",
  "urls": [{ "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00457", "language": "SPANISH", "tags": ["default"] }],
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

<p class="common-fields-legend"><span class="swatch"></span> Las líneas resaltadas son los campos <strong>comunes</strong> heredados de la clase base (ver <a href="./campos-comunes.md">campos-comunes.md</a>).</p>

</div>

---

## Reglas de validación

- Si `state` = `PENDING_TO_BE_READ_BY_DESTINATION`, entonces `readAt` debe ser `null`.
- Si `state` = `ACKNOWLEDGED_BY_DESTINATION` o `REJECTED_BY_DESTINATION`, entonces `readAt` debe tener valor.
- `issuedAt` debe ser anterior o igual a `readAt` (si existe).
- Ver [validaciones.md](../validaciones.md) para reglas completas.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
