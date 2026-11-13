# Auditoría de documentación DENA v0.5.20

**Fecha:** 2026-11-12  
**Alcance:** Alineamiento documentación ↔ código 0.5.20  
**Versión del Core:** Factory AI Core 2.23.0  
**Módulos auditados:**
- `e80a021-dena-common/e80a021-dena-common-data-api/` — modelos de datos
- `e80a021-dena-common/e80a021-dena-common-api/` — API común, tipos de mensaje, headers
- `e80a021-dena-common/e80a021-dena-common-interop-api/` — headers de interop
- `e80a021a-dena-interop/` — endpoints REST de sync/retrieve
- `e80a021g-dena-interop-connectors/` — conector base
- `e80a021-dena-common/e80a021-dena-common-docs/docs/` — documentación auditada

---

## Resumen ejecutivo

| Severidad | Cantidad | Descripción |
|-----------|:--------:|-------------|
| 🔴 CRÍTICO | 2 | Discrepancias que causan fallos de integración si se sigue la doc |
| 🟠 ALTO | 4 | Errores claros con alto potencial de confusión o malimplementación |
| 🟡 MEDIO | 5 | Inconsistencias menores o gaps documentales |
| 🔵 INFO | 2 | Observaciones sin impacto funcional directo |
| **TOTAL** | **13** | |

Los hallazgos más urgentes son:
1. **H01** — `autenticacion/administracion-core-dena/index.md` describe erróneamente los algoritmos y formato del digest de seguridad: provocaría que la administración genere firmas incorrectas.
2. **H02** — `index.md` de data-retrieve muestra nombres de constante Java (`ADMINISTRATIVE_RECORD`) en lugar del valor real JSON (`administrativeServiceProcedureRecord`): cualquier implementación que siga esa tabla enviará un `dataType.id` inválido.

---

## Hallazgos por área

---

### Área 1 · DataType IDs (`DN00DataTypeEnum`)

**H02 — ALTO · Observado**

#### Código fuente (referencia)
`e80a021-dena-common/e80a021-dena-common-data-api/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DataTypeEnum.java`

```java
ADMINISTRATIVE_NOTICE("administrativeNotice"),
ADMINISTRATIVE_RECORD("administrativeServiceProcedureRecord"),
PAYMENT_ONE_OFF_PAYMENT("oneOffPayment"),
PAYMENT_DIRECT_DEBIT_PAYMENT("directDebitPayment"),
SCHEDULE("scheduleItem"),
PERSON_DATA("personData"),
...
```

Los valores de `_marshallTypeId` son los que viajan en el JSON como `dataType.id`.

#### Discrepancia detectada

En `semantica/data-retrieve/index.md` (sección "Tipos de dato (`dataType.id`)"), la tabla usa **nombres de constante Java** como si fueran los valores JSON:

| Campo en doc | Valor doc (INCORRECTO) | Valor código correcto | Evidencia |
|---|---|---|---|
| `dataType.id` para Expediente | `ADMINISTRATIVE_RECORD` | `administrativeServiceProcedureRecord` | `DN00DataTypeEnum.java` |
| `dataType.id` para Notificación | `ADMINISTRATIVE_NOTICE` | `administrativeNotice` | `DN00DataTypeEnum.java` |
| `dataType.id` para Pago único | `PAYMENT_ONE_OFF_PAYMENT` | `oneOffPayment` | `DN00DataTypeEnum.java` |
| `dataType.id` para Domiciliación | `PAYMENT_DIRECT_DEBIT_PAYMENT` | `directDebitPayment` | `DN00DataTypeEnum.java` |
| `dataType.id` para Cita | `SCHEDULE` | `scheduleItem` | `DN00DataTypeEnum.java` |
| `dataType.id` para Persona | `PERSON_DATA` | `personData` | `DN00DataTypeEnum.java` |
| `dataType.id` para Credencial | `PERSON_CREDENTIAL` | `personCredential` | `DN00DataTypeEnum.java` |
| `dataType.id` para Credencial euskera | `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | `personCredentialForBasqueLanguageDegree` | `DN00DataTypeEnum.java` |
| `dataType.id` para Beneficio | `ECONOMIC_BENEFIT` | `economicBenefit` | `DN00DataTypeEnum.java` |
| `dataType.id` para Libro registro | `ADMINISTRATIVE_LEDGER` | `administrativeLedger` | `DN00DataTypeEnum.java` |
| `dataType.id` para Registro persona | `ADMINISTRATIVE_PERSON_REGISTER` | `administrativePersonRegister` | `DN00DataTypeEnum.java` |
| `dataType.id` para PEV vivienda | `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | `administrativePersonRegisterForHousingPEV` | `DN00DataTypeEnum.java` |
| `dataType.id` para Últimas voluntades | `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | `administrativePersonRegisterForLastWill` | `DN00DataTypeEnum.java` |
| `dataType.id` para Revisión PEV | `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | `administrativePersonRegisterForRevisionPEV` | `DN00DataTypeEnum.java` |
| `dataType.id` para Matrícula | `PERSON_ENROLLMENT` | `personEnrollment` | `DN00DataTypeEnum.java` |
| `dataType.id` para Matrícula académica | `PERSON_ACADEMIC_ENROLLMENT` | `personAcademicEnrollment` | `DN00DataTypeEnum.java` |
| `dataType.id` para Datos médicos | `PERSON_MEDICAL_DATA` | `personMedicalData` | `DN00DataTypeEnum.java` |

**Nota:** La tabla en `semantica/semantica-base/modelo/data-type-ref.md` y la tabla en `semantica/data-retrieve/endpoint-data-retrieve.md` sí usan los valores correctos (`administrativeServiceProcedureRecord`, `administrativeNotice`, etc.). El error está **solo** en la tabla resumen de `index.md`.

**Impacto:** Una administración que consulte únicamente el index.md enviará `dataType.id = "ADMINISTRATIVE_RECORD"` en lugar de `"administrativeServiceProcedureRecord"`.

---

### Área 2 · Tipos de mensaje (`DN00InteropMessageType`)

**H03 — ALTO · Observado**

#### Código fuente (referencia)
`e80a021-dena-common/e80a021-dena-common-api/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageType.java`

#### Sub-hallazgo A — Sufijos `_REQ`/`_RESP` omitidos en `message-types.md`

`semantica/semantica-base/modelo/message-types.md` muestra muchos tipos sin los sufijos `_REQ`/`_RESP`:

| Valor documentado (incompleto) | Valores reales en código |
|---|---|
| `CLIENT_LOGIN` | `CLIENT_LOGIN_REQ`, `CLIENT_LOGIN_RESP` |
| `CLIENT_PASSKEY_REGISTER_INIT` / `_FINISH` | `CLIENT_PASSKEY_REGISTER_INIT_REQ`, `CLIENT_PASSKEY_REGISTER_INIT_RESP`, `CLIENT_PASSKEY_REGISTER_FINISH_REQ`, `CLIENT_PASSKEY_REGISTER_FINISH_RESP` |
| `CLIENT_PASSKEY_LOGIN_INIT` / `_FINISH` | `CLIENT_PASSKEY_LOGIN_INIT_REQ`, `CLIENT_PASSKEY_LOGIN_INIT_RESP`, `CLIENT_PASSKEY_LOGIN_FINISH_REQ`, `CLIENT_PASSKEY_LOGIN_FINISH_RESP` |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS` | `CLIENT_PASSKEY_CLEAN_CREDENTIALS_REQ`, `CLIENT_PASSKEY_CLEAN_CREDENTIALS_RESP` |
| `PERSON_FETCH_DATA` | `PERSON_FETCH_DATA_REQ`, `PERSON_FETCH_DATA_RESP` |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH` | `ADMIN_PERSON_PULL_BESPOKE_FETCH_REQ`, `ADMIN_PERSON_PULL_BESPOKE_FETCH_RESP` |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` | `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_REQ`, `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_RESP` |
| `PERSON_ADMIN_SEARCH` | `PERSON_ADMIN_SEARCH_REQ`, `PERSON_ADMIN_SEARCH_RESP` |
| `PERSON_ADMIN_HEAD` | `PERSON_ADMIN_HEAD_REQ`, `PERSON_ADMIN_HEAD_RESP` |

#### Sub-hallazgo B — Tipos del enum completamente ausentes en `message-types.md`

Los siguientes valores del enum **no aparecen** en la documentación:

- `CLIENT_REFRESH_TOKEN_REQ` / `CLIENT_REFRESH_TOKEN_RESP`
- `CLIENT_USER_DELETE_REQ` / `CLIENT_USER_DELETE_RESP`
- `CLIENT_USER_DELETE_BY_ID_REQ` / `CLIENT_USER_DELETE_BY_ID_RESP`
- `CLIENT_PERSON_FETCH_REQ` / `CLIENT_PERSON_FETCH_RESP`
- `CLIENT_PERSON_UPDATE_REQ` / `CLIENT_PERSON_UPDATE_RESP`
- `PERSON_PUSH_TO_ADMIN_REQ` / `PERSON_PUSH_TO_ADMIN_RESP`
- `ADMIN_LOGIN_REQ` / `ADMIN_LOGIN_RESP`
- `ADMIN_PERSON_PULL_PREGEN_FETCH_REQ` / `ADMIN_PERSON_PULL_PREGEN_FETCH_RESP`
- `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_RESP`
- `CLIENT_LOGIN_DEMO`

**H07 — MEDIO · Observado**

En `semantica/data-retrieve/endpoint-data-retrieve.md`, el JSON de ejemplo request y response usa `"type": "PERSON_FETCH_DATA"` (sin `_REQ`), mientras el enum tiene `PERSON_FETCH_DATA_REQ`:

```json
"message": { "type": "PERSON_FETCH_DATA", "correlationId": "550e8400..." }
```

El resto de la documentación (index.md) usa correctamente `CLIENT_RETRIEVE_REQ`/`CLIENT_RETRIEVE_RESP`. Este es el tipo correcto para el flujo data-retrieve desde la perspectiva del endpoint de la administración.

---

### Área 3 · Modelos de datos

#### 3a — DN00ScheduleItem — campo `"details"` fantasma

**H05 — ALTO · Observado**

En `semantica/data-retrieve/index.md` (líneas ~424-492), el ejemplo JSON de respuesta del sandbox incluye el campo `"details"` dentro de objetos `scheduleItem`:

```json
{
  "type": "scheduleItem",
  "details": {
    "SPANISH": "Título de familia numerosa",
    "BASQUE": "Título de familia numerosa"
  },
  "subject": {
    "SPANISH": "Título de familia numerosa",
    ...
  }
  ...
}
```

**Evidencia del código** (`DN00ScheduleItem.java`): La clase NO tiene ningún campo `_details`. Los únicos campos de tipo `LanguageTexts` son:

```java
// e80a021-dena-common/e80a021-dena-common-data-api/denaCommonDataAPIScheduleModelClasses
@MarshallField(as="subject", whenXml=@MarshallFieldAsXml(attr=true))
@Getter @Setter private LanguageTexts _subjectByLanguage;
// NO existe campo _details
```

Diagnóstico: El JSON de ejemplo proviene de datos reales del sandbox que incluyen un campo (`details`) que ya no existe en el modelo v0.5.20 o que nunca formó parte del modelo estándar. El ejemplo debe actualizarse.

#### 3b — DN00ScheduleItem — clave `"type"` duplicada en tabla `cita.md`

**H06 — MEDIO · Observado**

En `semantica/data-retrieve/data/cita.md`, la tabla de Atributos JSON tiene dos filas con la clave `type`:

| Campo | Tipo | Valor doc |
|---|---|---|
| `type` | `String` | `"scheduleItem"` (discriminador polimórfico) |
| `type` (DN00ScheduleItemType) | `String` | `"IN_PERSON_APPOINTMENT"` |

Y en el ejemplo JSON de la misma página:
```json
{
  "type": "scheduleItem",  ← primera aparición
  ...
  "type": "IN_PERSON_APPOINTMENT",  ← segunda aparición (JSON inválido con claves duplicadas)
  ...
}
```

**Evidencia del código**: En `DN00ScheduleItem.java` el campo `_type` (de tipo `DN00ScheduleItemType`) se serializa como `@MarshallField(as="type")`, que **coincide** con el discriminador de `@MarshallType(as="scheduleItem")`. El framework objectstreamer gestiona internamente la colisión (el tipo polimórfico es especial), pero la tabla y el ejemplo JSON resultan confusos y el JSON de ejemplo es técnicamente inválido (clave duplicada).

**Recomendación:** El campo `DN00ScheduleItemType` debería documentarse con el nombre JSON definitivo que produce objectstreamer (probablemente el discriminador toma `"type"` y el campo real usa otro nombre, o bien coexisten de forma diferente). Requiere verificación con el equipo de DENA.

#### 3c — Campos URL en ejemplos JSON

**H04 — ALTO · Observado**

En múltiples documentos los ejemplos JSON usan `"url"` y `"language"` para los ítems de la colección URLs, pero la clase `UrlCollectionItem` / `MetaDatedValuesCollectionItemBase` del framework r01f serializa:
- el campo del valor de la URL como **`"value"`** (`@MarshallField(as="value")` heredado de `MetaDatedValuesCollectionItemBase`)
- el campo del idioma como **`"lang"`** (`@MarshallField(as="lang")` en `UrlCollectionItem`)

**Evidencia del código**:
```java
// e80a076-fabric-r01f/.../MetaDatedValuesCollectionItemBase.java
@MarshallField(as="value")
@Getter @Setter protected T _value;

// e80a076-fabric-r01f/.../UrlCollectionItem.java
@MarshallField(as="lang", whenXml=@MarshallFieldAsXml(attr=true))
@Getter @Setter protected Language _language;
```

El JSON correcto (según el ejemplo del sandbox en `index.md` línea ~453):
```json
"urls": [
  { "id": "48D2", "tags": ["default"], "value": "https://sandbox.dena.eus/schedules" }
]
```

**Documentos afectados** (usan `"url"`/`"language"` en lugar de `"value"`/`"lang"`):

| Documento | Tipo de objeto | Línea aprox. | Texto incorrecto |
|---|---|---|---|
| `semantica/data-retrieve/data/cita.md` | scheduleItem | ~125 | `{ "url": "https://...", "language": "SPANISH", "tags": ["default"] }` |
| `semantica/data-retrieve/data/libro-registro.md` | administrativeLedger | tabla `urls` | `"url": "https://...", "language": "SPANISH"` |
| `semantica/data-retrieve/endpoint-data-retrieve.md` | expediente | ~100 | `{ "url": "https://...", "language": "SPANISH", "tags": ["default"] }` |
| `semantica/data-retrieve/endpoint-data-retrieve.md` | notificación | ~186 | `[{ "url": "...", "language": "SPANISH", "tags": ["default"] }]` |
| `semantica/data-retrieve/endpoint-data-retrieve.md` | cita | ~245 | (igual) |

**Nota:** `semantica/data-retrieve/data/campos-comunes.md` usa correctamente `"value"` y `"lang"` en su ejemplo.

#### 3d — DN00PersonData extiende `NOT_IDENTIFIED_OBJECT` (sí documentado)

Sin discrepancia. La documentación en `persona.md` advierte correctamente que `DN00PersonData` no tiene `oid` ni `id` y que extiende `DN00DENADataExchangedNOTIdentifiedObject`. El código confirma:

```java
// denaCommonDataAPIPersonDataModelClasses
public class DN00PersonData
     extends DN00DENADataExchangedNOTIdentifiedObject<DN00PersonData>
```

---

### Área 4 · Campos comunes (`DN00DENADataExchangedObjectBase`)

Sin discrepancias relevantes. La documentación `campos-comunes.md` describe correctamente los campos:
- `oid`, `id` → heredados de `DN00DENADataExchangedObjectBase`
- `originAdmin`, `aboutPerson`, `urls`, `lastChangedAt` → heredados de `DN00DENADataExchangedNOTIdentifiedObject`

El código confirma:
```java
// DN00DENADataExchangedNOTIdentifiedObject.java
@MarshallField(as="originAdmin") DN00OrgAdminRef _originAdmin;
@MarshallField(as="aboutPerson") DN00PersonRef _aboutPerson;
@MarshallField(as="urls")        UrlCollection _urls;
@MarshallField(as="lastChangedAt") Instant _lastChangedAt;

// DN00DENADataExchangedObjectBase.java
@MarshallField(as="oid") O _oid;
@MarshallField(as="id")  I _id;
```

---

### Área 5 · Endpoints REST

**Evidencia del código** para los endpoints documentados:

| Endpoint doc | Path código | Clase | Estado |
|---|---|---|---|
| `POST /api/retrieveData` | `POST /api/connector/retrieveData` | `DN01ConnectorController.java` L50 | ✅ Consistente (la doc aclara que la ruta es configurable) |
| `POST /api/admin/interop/sync/metadata` | `@RequestMapping("api/admin/interop/sync")` + `@PostMapping("/metadata")` = `api/admin/interop/sync/metadata` | `DN01RESTInteropMetadataAdminSyncResource.java` L28, L60 | ✅ Consistente |

Sin discrepancias en rutas de endpoint.

---

### Área 6 · Headers de seguridad

**H01 — CRÍTICO · Observado**

#### Código fuente (referencia)
`e80a021-dena-common/e80a021-dena-common-api/denaCommonAPIClasses/src/main/java/dena/common/utils/DN00ClientApiRestUtilsHeaderDigestGenerator.java`

```java
/**
 * X-DENA-Data-Digest = {algorithm}=:base64(hash(timestamp + correlationId + body)):
 * Content-Digest     = {algorithm}=:base64(hash(body)):
 */
public static String generateDenaContentDigest(..., String timestamp, String correlationId, String body) {
    return _digest(algorithm, timestamp + correlationId + body);
}
public static String generateContentDigest(..., String body) {
    return _digest(algorithm, body);
}
```

El formato del valor del header es (de `DN01DENAHeadersConstants.java`):
```java
public static final String DIGEST_HEADER_DELIMITER = "=:";  // after algorithm
public static final String DIGEST_HEADER_SUFFIX = ":";       // after hash
// → formato completo: "SHA-256=:<base64>:"
```

#### Discrepancias en `autenticacion/administracion-core-dena/index.md`

| Campo | Valor documentado (INCORRECTO) | Valor correcto (código) |
|---|---|---|
| Algoritmo de `X-DENA-Data-Digest` | "Hash SHA-256 del body del mensaje" | SHA-256(timestamp + correlationId + body) |
| Algoritmo de `Content-Digest` | "Hash SHA-256 del mensaje completo (headers + body)" | SHA-256(body only) |
| Formato del header | `sha-256=2251f5ac60e4...` | `SHA-256=:<base64>:` |

**Comparativa entre documentos**:

| Header | `http-headers.md` (correcto) | `administracion-core-dena/index.md` (incorrecto) |
|---|---|---|
| `X-DENA-Data-Digest` | Hash(timestamp + correlationId + body) | "Hash del body" |
| `Content-Digest` | Hash(body only) | "Hash del mensaje completo (headers + body)" |
| Formato | `SHA-256=:<base64>:` | `sha-256=2251f5ac60e4...` |
| `X-DENA-Origin-TimeStamp` | **Obligatorio** (DENA exige en el warning) | **Opcional** |

`http-headers.md` es el documento de referencia correcto y coincide con el código.

**Impacto:** Una administración que siga `administracion-core-dena/index.md` para calcular los digests generará firmas incorrectas y recibirá `HTTP 401`.

**H10 — MEDIO · Observado**

`DN01DENAHeadersConstants.java` (módulo interno `denaCommonAPIClasses`) define una constante adicional:
```java
public static final String HEADER_ORG_ADMIN_ID = "X-DENA-ORG-ADMIN-ID";
```

Este header **no aparece** en:
- `semantica/semantica-base/http-headers.md`
- `autenticacion/administracion-core-dena/index.md`

El módulo público `DN00InteropHeaders.java` tampoco lo incluye. No se puede determinar con evidencia de repositorio si este header es activo o experimental. ⚠️ SIN VERIFICAR su uso real.

---

### Área 7 · Versiones y dependencias

**H08 — MEDIO · Observado**

`mkdocs-vars.yaml` (línea 17-18) documenta explícitamente el siguiente pendiente:

```yaml
# PENDIENTE (no actualizar hasta que exista el tag):
#   - conx_test_blob/conx_test_tree -> siguen en PUBLIC-v0.4.16. Tag PUBLIC-v0.5.20 sin verificar
```

```yaml
conx_test_tree: "https://github.com/DENA-Euskadi/dena-admin-conx-test/tree/PUBLIC-v0.4.16"
conx_test_blob: "https://github.com/DENA-Euskadi/dena-admin-conx-test/blob/PUBLIC-v0.4.16"
```

Todos los demás repos apuntan a `PUBLIC-v0.5.20`. Las URLs de este repositorio de test del conector (`dena-admin-conx-test`) siguen apuntando a v0.4.16. Los documentos que referencien ejemplos o código de este repo mostrarán una versión anterior.

El pendiente está documentado pero sin fecha de resolución.

---

### Área 8 · Snippets de código Java en la documentación

**H09 — MEDIO · Observado**

En `operativas/metadata-sync.md`, el snippet Java del paso 2 usa un valor de `dataTypeId` que no es un `marshallTypeId` válido del catálogo:

```java
new DN00DataTypeRef(DN00DataTypeID.forId("citizen_service_appointment"))
```

El valor `"citizen_service_appointment"` no existe en `DN00DataTypeEnum.java`. Los valores válidos son `"scheduleItem"`, `"administrativeNotice"`, etc. Este es claramente un valor de ejemplo inventado (placeholder), pero puede confundir a integradores.

El snippet de `operativas/data-retrieve.md` usa correctamente `DN00ScheduleItemID.forId(item.id())` y los métodos de `DN00ScheduleItem` — todos confirmados en el código.

---

### Área 9 · Estructura de directorios / packages

**H11 — MEDIO · Observado**

`DN00InteropMessageType.java` existe en dos paquetes distintos:

| Módulo | Paquete |
|---|---|
| `e80a021-dena-common/e80a021-dena-common-api/` (privado/interno) | `dena.api.common.interop.context` |
| `public/dena-common-api/` (público, al que apuntan los repos en `mkdocs-vars.yaml`) | `dena.api.common.model.interop.context` |

La documentación referencia la URL del repositorio público (`common_api_blob`), donde el paquete es diferente del que usa el código interno del monorepo. Esto podría confundir a integradores que consulten el código del repo público.

La misma discrepancia se confirma para `DN00InteropHeaders.java`:
- Interno: no localizado en `e80a021-dena-common`
- Público: `public/dena-common-interop-api/...dena/api/common/model/interop/DN00InteropHeaders.java` (paquete `dena.api.common.model.interop`)

---

### Área 10 · Tipos de datos adicionales verificados

Verificados contra código sin discrepancias:

| Tipo | Clase Java | MarshallType | Estado |
|---|---|---|---|
| Expediente | `DN00AdmistrativeServiceProcedureRecord` | `administrativeServiceProcedureRecord` | ✅ |
| Notificación | `DN00AdministrativeNotice` | `administrativeNotice` | ✅ |
| Cita | `DN00ScheduleItem` | `scheduleItem` | ✅ |
| Persona | `DN00PersonData` | `personData` | ✅ |
| Pago único | `DN00OneOffPayment` | ⚠️ SIN VERIFICAR (no localizado en módulo privado) | |
| Domiciliación | `DN00DirectDebitPayment` | ✅ `directDebitPayment` | ✅ |
| Beneficio económico | `DN00EconomicBenefit` | `economicBenefit` | ✅ |
| Libro registro | `DN00AdministrativeLedger` | `administrativeLedger` | ✅ |
| Registro persona (base) | `DN00PersonEnrollmentBase` (para `PERSON_ENROLLMENT`) | `personEnrollment` | ✅ |
| Matrícula académica | `DN00PersonAcademicEnrollment` | `personAcademicEnrollment` | ✅ |
| Credencial persona | `DN00PersonCredential` | `personCredential` | ✅ |
| Credencial euskera | `DN00PersonCredentialForBasqueLanguageDegree` | `personCredentialForBasqueLanguageDegree` | ✅ |
| Registro PEV vivienda | `DN00AdministrativePersonRegisterForHousingPEV` | `administrativePersonRegisterForHousingPEV` | ✅ |
| Registro últimas voluntades | `DN00AdministrativePersonRegisterForLastWill` | `administrativePersonRegisterForLastWill` | ✅ |
| Registro revisión PEV | `DN00AdministrativePersonRegisterForRevisionPEV` | `administrativePersonRegisterForRevisionPEV` | ✅ |
| Datos médicos | `DN00PersonMedicalData` | `personMedicalData` | ✅ |

---

## Hallazgos informativos

**H12 — INFO · ⚠️ SIN VERIFICAR**

En `index.md`, el JSON del sandbox para `scheduleItem` muestra los campos de fecha y hora como **strings**:
```json
"dayOfMonth": "20",  ← string
"hourOfDay": "10",   ← string
"durationMinutes": 30  ← number (correcto)
```

La documentación `cita.md` los describe como `Number`. Los tipos Java son `DayOfMonth`, `MonthOfYear`, etc. que son wrappers — su serialización exacta como string o number depende del TypeAdapter de Gson configurado. No se pudo determinar con evidencia directa de repositorio si el sandbox estaba serializado con un adaptador diferente.

**H13 — INFO · Observado**

El nombre de la clase `DN00AdmistrativeServiceProcedureRecord` tiene un typo histórico (falta la 'n' de "Administrative" → "Admistrative"). Este typo está **presente en el código** y es consistente con la documentación. No es una discrepancia doc↔código, pero sí un code smell que afecta a todos los puntos de la API que referencian esta clase.

La clase existe con este nombre en:
- `e80a021-dena-common/e80a021-dena-common-data-api/.../administrativeservices/DN00AdmistrativeServiceProcedureRecord.java`
- Referenciada en `DN00AdministrativeNotice.java`, `DN00ScheduleItem.java`, `DN00AdministrativeLedger.java`, etc.

---

## Pendientes de verificar (⚠️ SIN VERIFICAR)

Los siguientes puntos no pudieron corroborarse con evidencia directa del repositorio en esta auditoría:

| ID | Área | Descripción | Qué falta para verificar |
|---|---|---|---|
| PV-01 | Área 3b | Comportamiento real de objectstreamer cuando `@MarshallType` y un `@MarshallField` usan el mismo nombre JSON `"type"` en `DN00ScheduleItem` | Inspeccionar serialización real o tests de integración |
| PV-02 | Área 3 | `DN00OneOffPayment` no localizado en el módulo privado `e80a021-dena-common-data-api` | El repo público tiene `denaCommonDataAPIPaymentModelClasses/...DN00OneOffPayment.java`; verificar que el módulo privado es equivalente |
| PV-03 | Área 6 | `X-DENA-ORG-ADMIN-ID` — si este header interno (`DN01DENAHeadersConstants.HEADER_ORG_ADMIN_ID`) se usa activamente en algún flujo de interoperabilidad | Revisar usos de la constante en el módulo CORE |
| PV-04 | Área 8 | Tipado JSON de `DayOfMonth`, `MonthOfYear`, `HourOfDay`, `MinuteOfHour` como string vs number | Revisar la configuración del ObjectStreamer y los tests del sandbox |
| PV-05 | Área 7 | Tag `PUBLIC-v0.5.20` en el repositorio `dena-admin-conx-test` | Verificar si el tag fue publicado posteriormente al corte de esta auditoría |

---

## Conclusiones y recomendaciones

### Acciones urgentes (bloquean integración)

1. **Corregir `autenticacion/administracion-core-dena/index.md`** (H01):
   - `X-DENA-Data-Digest` = SHA-256(timestamp + correlationId + body), formato `SHA-256=:<base64>:`
   - `Content-Digest` = SHA-256(body), mismo formato
   - `X-DENA-Origin-TimeStamp` es **obligatorio** (alinear con `http-headers.md`)
   - Referencia correcta: `DN00ClientApiRestUtilsHeaderDigestGenerator.java`

2. **Corregir tabla de tipos en `index.md`** (H02):
   - Sustituir los nombres de constante Java (`ADMINISTRATIVE_RECORD`) por los valores JSON correctos (`administrativeServiceProcedureRecord`)
   - Referenciar o enlazar a `data-type-ref.md` que ya tiene la tabla correcta

### Acciones de prioridad alta

3. **Corregir campos `"url"`/`"language"` por `"value"`/`"lang"` en los ejemplos JSON** (H04) en: `cita.md`, `libro-registro.md` y múltiples ejemplos de `endpoint-data-retrieve.md`.

4. **Eliminar o documentar el campo `"details"` en el ejemplo de scheduleItem** de `index.md` (H05). Si fue eliminado del modelo, actualizar el JSON de ejemplo al estado real del sandbox.

5. **Completar `message-types.md`** (H03): añadir todos los tipos `_REQ`/`_RESP` faltantes y los ~18 tipos de mensaje no documentados.

### Acciones de prioridad media

6. **Corregir el tipo de mensaje en `endpoint-data-retrieve.md`** (H07): usar `PERSON_FETCH_DATA_REQ` o verificar con el equipo si el nombre canónico para este endpoint es diferente.

7. **Resolver la tabla duplicada de `type` en `cita.md`** (H06): aclarar cómo serializa objectstreamer el discriminador polimórfico junto al campo `_type`.

8. **Actualizar `mkdocs-vars.yaml`** con el tag de `conx_test` cuando esté disponible (H08).

9. **Corregir el valor de ejemplo `"citizen_service_appointment"`** en `operativas/metadata-sync.md` por un valor real del catálogo, p.ej. `"scheduleItem"` (H09).

### Acciones de prioridad baja

10. **Documentar `X-DENA-ORG-ADMIN-ID`** en `http-headers.md` si este header es parte del contrato público (H10).

11. **Documentar la diferencia de paquetes** entre módulo interno y público de `DN00InteropMessageType` (H11), o garantizar que el módulo público refleja el interno.

---

*Informe generado con asistencia de Kiro — revisión humana obligatoria antes de actuar sobre los hallazgos.*

<!-- audit-footer -->
---
<sub>Auditoría DENA docs v0.5.20 · Kiro + maintenance-agent · 2026-11-12</sub>
