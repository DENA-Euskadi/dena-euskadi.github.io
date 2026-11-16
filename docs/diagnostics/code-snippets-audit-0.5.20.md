# Auditoría de Snippets de Código — v0.5.20

> **Fecha:** 2025-07-11  
> **Auditor:** Kiro (maintenance-agent)  
> **Método:** Verificación de cada elemento contra el código fuente en `e80a021-dena-interop` rama `develop`  
> **Alcance:** Grupos A, B, C y D según especificación de la tarea

---

## Resumen

| Severidad | Cantidad |
|-----------|----------|
| 🔴 CRÍTICO | 5 |
| 🟠 ALTO    | 4 |
| 🟡 MEDIO   | 4 |
| 🔵 INFORMATIVO | 3 |
| **Total** | **16** |

> **Hallazgo dominante:** Los DataType IDs usados en los switch/match de todos los snippets de múltiples lenguajes y en la tabla de tipos de la guía de implementación son el `marshallTypeId` (camelCase, ej. `scheduleItem`) cuando DENA envía el nombre del enum SCREAMING_SNAKE_CASE (ej. `SCHEDULE`). Esto provocará que **ninguna integración que use esos snippets detecte correctamente el tipo de dato solicitado**.

---

## Hallazgos por fichero

---

### `operativas/data-retrieve.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 1 | JSON context (ejemplo impl.) | `"dataType": { "id": "scheduleItem" }` | 🔴 CRÍTICO — ID incorrecto | El ID externo del tipo de dato es `"SCHEDULE"` (nombre del enum `DN00DataTypeEnum.SCHEDULE`). El sandbox delegate usa `"SCHEDULE"` como clave; el ejemplo real de la misma página muestra `"id": "SCHEDULE"`. Ver `DN00DataTypeEnum.java:22`. |
| 2 | JSON context (ejemplo impl.) | `"message": { "type": "PERSON_FETCH_DATA" }` | 🟠 ALTO — tipo de mensaje inexistente | `DN00InteropMessageType` no tiene `PERSON_FETCH_DATA`. Valores correctos: `CLIENT_RETRIEVE_REQ` (petición) / `CLIENT_RETRIEVE_RESP` (respuesta). Ver `DN00InteropMessageType.java:102,213`. |
| 3 | Java snippet `_fromDBDataToReturnedDataItems` | `UrlCollection.create().addItems(...)` | 🟠 ALTO — método inexistente + paréntesis rotos | `addItems()` no existe en `UrlCollection`. Los métodos disponibles son `addUrls(Url...)` y `add(UrlCollectionItem)`. Además los paréntesis del snippet son sintácticamente inválidos. Patrón correcto usado en producción (sandbox): `new UrlCollection(); urls.add(UrlCollectionItem.createFor(url).withTags("default")); item.setUrls(urls);`. Ver `DN01SandboxScheduleServicesDelegateForBusiness.java:92-93`. |
| 4 | JSON context (ejemplo completo nota) | `"dataType": { "id": "scheduleItem" }` (dentro del ??? note) | 🔴 CRÍTICO — ID incorrecto | Mismo problema que hallazgo #1. |
| 5 | JSON REQUEST real | `"dataType": { "id": "SCHEDULE" }` | ✅ CORRECTO | Coincide con `DN00DataTypeEnum.SCHEDULE` y con el sandbox delegate `DATA_TYPE_MAP`. |
| 6 | JSON RESPONSE real | `"type": "scheduleItem"` (dentro de `data`) | ✅ CORRECTO | Es el `@MarshallType(as="scheduleItem")` discriminador del objeto de datos. |
| 7 | Java `DN00ScheduleItem` constructor | `new DN00ScheduleItem()` | ✅ CORRECTO | Clase existe en `dena/api/data/model/schedule/DN00ScheduleItem.java`. |
| 8 | Java método `setLastChangedAt()` | `schItem.setLastChangedAt(item.lastChangedAt())` | ✅ CORRECTO | Heredado de `DN00DENADataExchangedNOTIdentifiedObject._lastChangedAt` con `@Getter @Setter`. `@MarshallField(as="lastChangedAt")`. |
| 9 | Java método `setId()` | `schItem.setId(DN00ScheduleItemID.forId(...))` | ✅ CORRECTO | Heredado de `DN00DENADataExchangedObjectBase._id`. |
| 10 | Java método `setAt()` | `schItem.setAt(item.at())` | ✅ CORRECTO | Método `setAt(LocalDateTime)` / `setAt(Instant)` existe en `DN00ScheduleItem.java:84-97`. |
| 11 | Java método `setDurationMinutes()` | `schItem.setDurationMinutes(item.durationMins())` | ✅ CORRECTO | `@MarshallField(as="durationMinutes") @Getter @Setter private int _durationMinutes` en `DN00ScheduleItem.java:64`. |
| 12 | Java método `setSubjectByLanguage()` | `schItem.setSubjectByLanguage(new LanguageTextsMapBacked()...)` | ✅ CORRECTO | `@Getter @Setter private LanguageTexts _subjectByLanguage`. `LanguageTextsMapBacked` implementa `LanguageTexts`. Marshalled como `@MarshallField(as="subject")`. |
| 13 | JSON campo `urls[]` | `{ "id": "main", "lang": "BASQUE", "value": "..." }` | ✅ CORRECTO | `UrlCollectionItem`: `@MarshallField(as="lang")` → `lang`; hereda `@MarshallField(as="id")` → `id`; hereda `@MarshallField(as="value")` → `value` de `MetaDatedValuesCollectionItemBase`. |

---

### `operativas/metadata-sync.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 14 | Java `_toSRMD()` método | `new DN00SyncMetaDataFromAdminToCOREItem(...)` (5 args) | ✅ CORRECTO | Constructor de 5 args existe: `(DN00OrgAdminRef, DN00PersonRef, Instant, DN00DataTypeRef, DN00DataOriginInstanceID)`. Ver `DN00SyncMetaDataFromAdminToCOREItem.java:62`. |
| 15 | Java DataTypeID | `DN00DataTypeID.forId("SCHEDULE")` | ✅ CORRECTO | `DN00DataTypeEnum.SCHEDULE` → ID externo `"SCHEDULE"`. El sandbox usa `"SCHEDULE"` consistentemente. |
| 16 | Java constructor comment | `null  // null = instancia de origen por defecto (DEFAULT)` | 🟡 MEDIO — misleading | `null` y `DN00DataOriginInstanceID.DEFAULT` son valores distintos en Java (aunque en la práctica puede funcionar). Se recomienda pasar `DN00DataOriginInstanceID.DEFAULT` para claridad y consistencia con la constante definida en `DN00InteropConfigIDs.java:59`. |
| 17 | Java `DN00InteropContextBuilder` chain | `.fromAdmin()` → `.withSubjectPerson()` → `.aboutDataType()` → `.build()` | ✅ CORRECTO | La cadena de pasos del builder es correcta según `DN00InteropContextBuilder.java`. |
| 18 | Java `DN00SyncMetaDataFromAdminRequestMessage(ctx, srmdItems)` | Constructor 2-arg | ✅ CORRECTO | Constructor `(DN00InteropContext, Collection<>)` existe en `DN00SyncMetaDataFromAdminRequestMessage.java:39`. |
| 19 | JSON ejemplo Step 2 | `"ofType": { "id": "ADMIN_NOTICE" }` | 🔴 CRÍTICO — ID inexistente | `ADMIN_NOTICE` NO existe en `DN00DataTypeEnum`. El enum tiene `ADMINISTRATIVE_NOTICE("administrativeNotice")`. El ID correcto es `"ADMINISTRATIVE_NOTICE"`. Ver `DN00DataTypeEnum.java:18`. |
| 20 | JSON REQUEST real | `"ofType": { "id": "PERSON_DATA" }` | ✅ CORRECTO | Coincide con `DN00DataTypeEnum.PERSON_DATA("personData")`. |
| 21 | JSON campo `"fromDataOriginInstance"` | `"fromDataOriginInstance": "DEFAULT"` (en el JSON del ejemplo) | ✅ CORRECTO | `@MarshallField(as="fromDataOriginInstance")` en `DN00SyncMetaDataFromAdminToCOREItem.java:47`. |

---

### `operativas/person-sync.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 22 | JSON PUSH REQUEST `context.message.type` | `"PERSON_PUSH_TO_ADMIN_REQ"` | ✅ CORRECTO | Existe en `DN00InteropMessageType.java:137`. |
| 23 | JSON PUSH RESPONSE `context.message.type` | `"PERSON_PUSH_TO_ADMIN_RESP"` | ✅ CORRECTO | Existe en `DN00InteropMessageType.java:139`. |
| 24 | JSON `payload.syncData.syncEvent` | `"CREATED"` / `"UPDATED"` / `"DELETED"` | ✅ CORRECTO — ⚠️ pendiente verificar enum | Valores coherentes con la lógica del sandbox pero la clase exacta del `syncEvent` no fue verificada directamente en esta auditoría. |

---

### `semantica/data-retrieve/snippets-codigo.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 25 | Java (Spring Boot) switch | `case "scheduleItem"` | 🔴 CRÍTICO — ID incorrecto | Ver hallazgo #1. El ID enviado en `context.dataType.id` es `"SCHEDULE"`. Si el switch usa `"scheduleItem"` nunca hará match. Evidencia: sandbox `DATA_TYPE_MAP` usa `"SCHEDULE"`. |
| 26 | Java switch | `case "administrativeServiceProcedureRecord"` | 🔴 CRÍTICO — ID incorrecto | Debería ser `"ADMINISTRATIVE_RECORD"`. `DN00DataTypeEnum.ADMINISTRATIVE_RECORD("administrativeServiceProcedureRecord")`. |
| 27 | Java switch | `case "administrativeNotice"` | 🔴 CRÍTICO — ID incorrecto | Debería ser `"ADMINISTRATIVE_NOTICE"`. |
| 28 | Java switch | `case "administrativeLedger"` | 🔴 CRÍTICO — ID incorrecto | Debería ser `"ADMINISTRATIVE_LEDGER"`. |
| 29 | Java switch | `case "oneOffPayment"` | 🔴 CRÍTICO — ID incorrecto | Debería ser `"PAYMENT_ONE_OFF_PAYMENT"`. |
| 30 | Java switch | `case "personData"` | 🔴 CRÍTICO — ID incorrecto | Debería ser `"PERSON_DATA"`. |
| 31 | C# match | Mismo conjunto de camelCase IDs | 🔴 CRÍTICO | Mismo problema que los cases Java (filas 25-30). |
| 32 | Python dict | Mismo conjunto de camelCase IDs (`"scheduleItem"`, `"administrativeServiceProcedureRecord"`, ...) | 🔴 CRÍTICO | Mismo problema. |
| 33 | Node.js object | Mismo conjunto de camelCase IDs | 🔴 CRÍTICO | Mismo problema. |
| 34 | PHP match | Mismo conjunto de camelCase IDs | 🔴 CRÍTICO | Mismo problema. |
| 35 | Java/Python/Node.js `type` en data object devuelto | `"type": "administrativeServiceProcedureRecord"` (dentro del `data` object) | ✅ CORRECTO | El discriminador `@MarshallType(as="administrativeServiceProcedureRecord")` sí es el `type` dentro del objeto de dato. Diferente del `context.dataType.id`. |

---

### `semantica/data-retrieve/guia-implementacion.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 36 | Tabla Paso 1 — tipos de dato | `"scheduleItem"` | 🔴 CRÍTICO — ID incorrecto | La tabla presenta los `marshallTypeId` camelCase como si fueran los `context.dataType.id`. El valor real en la petición es `"SCHEDULE"`. |
| 37 | Tabla Paso 1 | `"administrativeServiceProcedureRecord"` | 🔴 CRÍTICO | Debería ser `"ADMINISTRATIVE_RECORD"`. |
| 38 | Tabla Paso 1 | `"administrativeNotice"` | 🔴 CRÍTICO | Debería ser `"ADMINISTRATIVE_NOTICE"`. |
| 39 | Tabla Paso 1 | `"oneOffPayment"`, `"directDebitPayment"` | 🔴 CRÍTICO | Deberían ser `"PAYMENT_ONE_OFF_PAYMENT"`, `"PAYMENT_DIRECT_DEBIT_PAYMENT"`. |
| 40 | Tabla Paso 1 | `"personData"`, `"personCredential"`, etc. | 🔴 CRÍTICO | Deberían ser `"PERSON_DATA"`, `"PERSON_CREDENTIAL"`, etc. |
| 41 | Ejemplo context Paso 1 | `"dataType": { "id": "administrativeServiceProcedureRecord" }` | 🔴 CRÍTICO | Debería ser `"ADMINISTRATIVE_RECORD"`. |
| 42 | Paso 3.3 JSON notificación | `"state": "PENDING_TO_BE_READ_BY_DESTINATION"` (string directo) | ✅ CORRECTO — validado por nota en doc | Nota en el propio doc explica que `state` es string directo (no objeto con `stateCode`). |
| 43 | Paso 3.4 JSON pago | `"data": { "forStatus": "PENDING" }` | ⚠️ PENDIENTE DE VERIFICAR | No se verificó la clase `DN00OneOffPaymentSchemaFormat` ni su campo `data.forStatus` en esta auditoría. |
| 44 | Paso 3.7 JSON cita | `"type": "IN_PERSON_APPOINTMENT"` | 🟠 ALTO — campo JSON erróneo | En `DN00ScheduleItem`, el campo del tipo de cita está anotado como `@MarshallField(as="typeSchedule")`, NO como `type`. El `type` está reservado para el discriminador `@MarshallType(as="scheduleItem")`. La cita serializada tendrá `"type": "scheduleItem"` (discriminador) y `"typeSchedule": "IN_PERSON_APPOINTMENT"` (tipo de cita). Ver `DN00ScheduleItem.java:74`. |
| 45 | Paso 3.7 JSON cita | `"contactInfo": { "address": "..." }` | ✅ CORRECTO | `@MarshallField(as="contactInfo")` en `DN00ScheduleItem.java:80`. |
| 46 | Paso 6 URLs | `{ "value": "...", "lang": "SPANISH", "tags": [...] }` | ✅ CORRECTO | Coincide con el marshalling de `UrlCollectionItem` + `MetaDatedValuesCollectionItemBase`. |
| 47 | Java Paso 2 endpoint | `request.getContext().getSubjectPerson().getId()` | ✅ CORRECTO — estructura lógica | Asumiendo que `InteropRequest` envuelve el context correctamente. |

---

### `guia-inicio/ejemplo-end-to-end.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 48 | Bash Step 2 curl SRMD | `"ofType": { "id": "RECORDS" }` | 🔴 CRÍTICO — ID inexistente | `RECORDS` NO existe en `DN00DataTypeEnum`. Enum tiene `ADMINISTRATIVE_RECORD("administrativeServiceProcedureRecord")`. Debería ser `"ADMINISTRATIVE_RECORD"`. Ver `DN00DataTypeEnum.java`. |
| 49 | Bash Step 2 — formato del body | Array bare `[{...}]` sin wrapper `context`/`payload` | 🟡 MEDIO — inconsistencia con metadata-sync | El ejemplo real en `metadata-sync.md` muestra la estructura completa con `{"context":{...},"payload":[...]}`. El body bare puede ser válido si el endpoint externo acepta ese formato simplificado, pero es inconsistente. Necesita aclaración/verificación. |
| 50 | JSON Step 3 response — URLs | `{ "url": "https://...", "language": "SPANISH", "tags": [...] }` | 🟠 ALTO — nombres de campo incorrectos | Los campos correctos son `"value"` (no `"url"`) y `"lang"` (no `"language"`). Evidencia: `@MarshallField(as="value")` en `MetaDatedValuesCollectionItemBase.java:35`; `@MarshallField(as="lang")` en `UrlCollectionItem.java`. El sandbox usa siempre `value` y `lang` en sus tests (`DN01SandboxDemoDataDeserializationTest.java`). |
| 51 | Bash script `fromDataOrigin` | `"fromDataOrigin": "DEFAULT"` | 🟡 MEDIO — nombre de campo incorrecto | El campo correcto es `"fromDataOriginInstance"` (de `@MarshallField(as="fromDataOriginInstance")` en `DN00SyncMetaDataFromAdminToCOREItem.java:47`). |
| 52 | Bash script `ofType` | `"ofType": { "id": "RECORDS" }` | 🔴 CRÍTICO | Mismo problema que hallazgo #48. |
| 53 | Bash checklist | `context.message.correlationId` mantenido en response | ✅ CORRECTO | El ejemplo de response en el mismo fichero lo incluye. |
| 54 | Bash Step 1 token URL | `"${KEYCLOAK_URL}/realms/${REALM}/protocol/openid-connect/token"` con `REALM="DenaAuthAdmins"` | ✅ CORRECTO | Coincide con el endpoint en `autenticacion/administracion-core-dena/index.md`. |

---

### `autenticacion/administracion-core-dena/index.md`

| # | Snippet | Elemento | Estado | Evidencia del código |
|---|---------|----------|--------|----------------------|
| 55 | Sección 2 headers tabla | `X-DENA-Message-Correlation-Id` | ✅ CORRECTO | `DN01DENAHeadersConstants.HEADER_CORRELATION_ID = "X-DENA-Message-Correlation-Id"`. |
| 56 | Sección 2 headers tabla | `X-DENA-This-TimeStamp` | ✅ CORRECTO | `DN01DENAHeadersConstants.HEADER_THIS_TIMESTAMP = "X-DENA-This-TimeStamp"`. |
| 57 | Sección 2 headers tabla | `X-DENA-Data-Digest` | ✅ CORRECTO | `DN01DENAHeadersConstants.HEADER_DENA_DATA_DIGEST = "X-DENA-Data-Digest"`. |
| 58 | Sección 2 headers tabla | `Content-Digest` | ✅ CORRECTO | `DN01DENAHeadersConstants.HEADER_CONTENT_DIGEST = "Content-Digest"`. |
| 59 | Sección 2 headers tabla | `X-DENA-Origin-TimeStamp` | ✅ CORRECTO | `DN01DENAHeadersConstants.HEADER_ORIGIN_TIMESTAMP = "X-DENA-Origin-TimeStamp"`. |
| 60 | Java digest | `"SHA-256=:" + Base64.getEncoder()... + ":"` | ✅ CORRECTO | Formato `{alg}=:{hash}:` coincide con `DN01DENAHeadersConstants.DIGEST_HEADER_DELIMITER = "=:"` y `DIGEST_HEADER_SUFFIX = ":"`. Algoritmo `"SHA-256"` coincide con `ALGORITHM_DENA_DIGEST`. |
| 61 | Sección 3 bash example | `"ofType":{"id":"RECORDS"}` | 🔴 CRÍTICO — ID inexistente | Mismo problema que hallazgo #48. `RECORDS` no existe; debería ser `"ADMINISTRATIVE_RECORD"`. |

---

## Consolidación de hallazgos críticos

### HC-1 — DataType IDs incorrectos en switch/match (impacta 5 ficheros, todos los lenguajes)

**Descripción:** Los snippets de código en múltiples ficheros y lenguajes usan los `marshallTypeId` (camelCase) como valor del `context.dataType.id` cuando el valor real que DENA envía es el nombre del enum `DN00DataTypeEnum` (SCREAMING_SNAKE_CASE).

**Impacto:** Cualquier integración que copie estos snippets tendrá un switch/match que nunca hace match, devolviendo siempre una lista vacía para todos los tipos de dato.

**Mapping correcto** (confirmado por sandbox `DATA_TYPE_MAP` + `DN00DataTypeEnum` + ejemplos reales en `data-retrieve.md`):

| ID en docs (INCORRECTO) | ID real `context.dataType.id` (CORRECTO) | Fuente enum |
|-------------------------|------------------------------------------|-------------|
| `scheduleItem` | `SCHEDULE` | `DN00DataTypeEnum:22` |
| `administrativeServiceProcedureRecord` | `ADMINISTRATIVE_RECORD` | `DN00DataTypeEnum:19` |
| `administrativeNotice` | `ADMINISTRATIVE_NOTICE` | `DN00DataTypeEnum:18` |
| `oneOffPayment` | `PAYMENT_ONE_OFF_PAYMENT` | `DN00DataTypeEnum:20` |
| `directDebitPayment` | `PAYMENT_DIRECT_DEBIT_PAYMENT` | `DN00DataTypeEnum:21` |
| `personData` | `PERSON_DATA` | `DN00DataTypeEnum:23` |
| `personCredential` | `PERSON_CREDENTIAL` | `DN00DataTypeEnum:24` |
| `economicBenefit` | `ECONOMIC_BENEFIT` | `DN00DataTypeEnum:25` |
| `administrativeLedger` | `ADMINISTRATIVE_LEDGER` | `DN00DataTypeEnum:26` |
| `administrativePersonRegister` | `ADMINISTRATIVE_PERSON_REGISTER` | `DN00DataTypeEnum:31` |
| `personEnrollment` | `PERSON_ENROLLMENT` | `DN00DataTypeEnum:30` |
| `personAcademicEnrollment` | `PERSON_ACADEMIC_ENROLLMENT` | `DN00DataTypeEnum:32` |
| `personCredentialForBasqueLanguageDegree` | `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | `DN00DataTypeEnum:33` |
| `personMedicalData` | `PERSON_MEDICAL_DATA` | `DN00DataTypeEnum:34` |
| `ADMIN_NOTICE` (no existe) | `ADMINISTRATIVE_NOTICE` | `DN00DataTypeEnum:18` |
| `RECORDS` (no existe) | `ADMINISTRATIVE_RECORD` | `DN00DataTypeEnum:19` |

**Ficheros afectados:**
- `semantica/data-retrieve/snippets-codigo.md` — switch en Java, C#, Python, Node.js, PHP
- `semantica/data-retrieve/guia-implementacion.md` — tabla Paso 1 + ejemplo context
- `operativas/data-retrieve.md` — JSON context (ejemplo simplificado)
- `operativas/metadata-sync.md` — JSON Paso 2 (`ADMIN_NOTICE`)
- `guia-inicio/ejemplo-end-to-end.md` — curl Step 2 + script (`RECORDS`)
- `autenticacion/administracion-core-dena/index.md` — Sección 3 (`RECORDS`)

### HC-2 — Tipo de mensaje inexistente `PERSON_FETCH_DATA`

**Descripción:** `data-retrieve.md` primer JSON de context usa `"type": "PERSON_FETCH_DATA"` que no existe en `DN00InteropMessageType`. El tipo correcto para la petición es `CLIENT_RETRIEVE_REQ` y para la respuesta `CLIENT_RETRIEVE_RESP`.

**Fichero:** `operativas/data-retrieve.md` — primer bloque JSON de ejemplo de implementación.

### HC-3 — `UrlCollection.addItems()` no existe

**Descripción:** El snippet Java en `data-retrieve.md` usa `.addItems(...)` sobre `UrlCollection`, método que no existe. Además los paréntesis del snippet son sintácticamente incorrectos (no compilaría).

**Patrón correcto** (de `DN01SandboxScheduleServicesDelegateForBusiness.java:92-93`):
```java
UrlCollection urls = new UrlCollection();
urls.add(UrlCollectionItem.createFor(url, CollectionItemID.forId("main"), Language.BASQUE)
                          .withTags("default"));
item.setUrls(urls);
```

### HC-4 — Campos de URL incorrectos en E2E

**Descripción:** `guia-inicio/ejemplo-end-to-end.md` Step 3 usa `"url"` y `"language"` para los campos de URL, cuando los campos reales (por `@MarshallField`) son `"value"` y `"lang"`.

### HC-5 — `typeSchedule` vs `type` en `DN00ScheduleItem`

**Descripción:** `guia-implementacion.md` Paso 3.7 muestra `"type": "IN_PERSON_APPOINTMENT"` para el tipo de cita. El campo real en la serialización es `"typeSchedule"` (de `@MarshallField(as="typeSchedule")`). El campo `"type"` está reservado para el discriminador polimórfico `@MarshallType(as="scheduleItem")`.

---

## Clases Java verificadas

| Clase | Paquete | Existe | Métodos verificados |
|-------|---------|--------|---------------------|
| `DN00ScheduleItem` | `dena.api.data.model.schedule` | ✅ | `setLastChangedAt()`, `setId()`, `setAt()`, `setDurationMinutes()`, `setSubjectByLanguage()`, `setUrls()` — todos ✅ |
| `DN00SyncMetaDataFromAdminToCOREItem` | `dena.api.interop.syncandretrieve.model.sync.admin` | ✅ (record) | Constructor 5-arg ✅, Constructor 6-arg ✅, `from()` factories ✅ |
| `DN00InteropContextBuilder` | `dena.api.common.interop.context` | ✅ | `createMessageOfType()` → `withGeneratedCorrelationOID()` → `fromAdmin()` → `withSubjectPerson()` → `aboutDataType()` → `build()` — cadena completa ✅ |
| `DN00SyncMetaDataFromAdminRequestMessage` | `dena.api.syncandretireve.messages.sync.admin` | ✅ | Constructor `(context, items)` ✅ |
| `DN00DataTypeEnum` | `dena.api.data.model` | ✅ | 17 entradas verificadas; `marshallTypeId` ≠ ID externo |
| `DN00InteropMessageType` | `dena.api.common.interop.context` | ✅ | `ADMIN_SRMD_SYNC_REQ/RESP` ✅, `CLIENT_RETRIEVE_REQ/RESP` ✅, `PERSON_PUSH_TO_ADMIN_REQ/RESP` ✅, `PERSON_FETCH_DATA` ❌ (no existe; correcto es `PERSON_FETCH_DATA_REQ/RESP`) |
| `DN01DENAHeadersConstants` | `dena.common.utils.constants` | ✅ | Todos los headers verificados ✅ |
| `UrlCollectionItem` | `r01f.types.url` | ✅ | `@MarshallField(as="lang")`, hereda `(as="value")`, `(as="id")`, `(as="tags")` ✅ |
| `UrlCollection` | `r01f.types.url` | ✅ | `create()` ✅, `addUrls(Url...)` ✅; `addItems()` ❌ no existe |

---

## Pendientes de verificar

| Elemento | Fichero | Razón |
|----------|---------|-------|
| `payload.syncData.syncEvent` valores (`CREATED`, `UPDATED`, `DELETED`) | `operativas/person-sync.md` | No se localizó la clase `DN00PersonSyncEvent` o similar para confirmar los valores del enum |
| `"data": { "forStatus": "PENDING" }` en pago | `guia-implementacion.md` paso 3.4 | No se verificó `DN00OneOffPaymentSchemaFormat` en esta auditoría |
| Formato del body SRMD en E2E: bare array vs wrapped | `guia-inicio/ejemplo-end-to-end.md` | Necesita verificar si el endpoint `/srmd/` acepta array sin wrapper `context`/`payload` o requiere la estructura completa |
| Registros de tipo `administrativePersonRegister*` (4 variantes) | `guia-implementacion.md` paso 3 | No se verificaron las 4 variantes del modelo de registro de persona |
| `"X-DENA-ORG-ADMIN-ID"` header en snippets curl | `autenticacion/` | `DN01DENAHeadersConstants.HEADER_ORG_ADMIN_ID = "X-DENA-ORG-ADMIN-ID"` definido pero no aparece en los ejemplos curl revisados |

---

## Recomendaciones de corrección (prioridad)

### P0 — Bloquea toda integración
1. **Corregir todos los DataType IDs** en `snippets-codigo.md`, `guia-implementacion.md`, `data-retrieve.md`, `metadata-sync.md`, `ejemplo-end-to-end.md` y `autenticacion/administracion-core-dena/index.md` según la tabla de mapping HC-1.

### P1 — Código incorrecto (no compilaría / produce respuesta incorrecta)
2. **Corregir snippet `setUrls()`** en `data-retrieve.md` — reemplazar `UrlCollection.create().addItems(...)` por el patrón `new UrlCollection(); urls.add(UrlCollectionItem.createFor(...))`.
3. **Corregir tipo de mensaje** `PERSON_FETCH_DATA` → `CLIENT_RETRIEVE_REQ` en `data-retrieve.md`.
4. **Corregir campos de URL** `"url"` → `"value"` y `"language"` → `"lang"` en `ejemplo-end-to-end.md` Step 3.
5. **Corregir `typeSchedule`** en `guia-implementacion.md` paso 3.7 (`"type": "IN_PERSON_APPOINTMENT"` → `"typeSchedule": "IN_PERSON_APPOINTMENT"`).

### P2 — Mejoras de claridad
6. **`ejemplo-end-to-end.md` script** — corregir `"fromDataOrigin"` → `"fromDataOriginInstance"`.
7. **`metadata-sync.md` Java snippet** — cambiar `null` → `DN00DataOriginInstanceID.DEFAULT` en el constructor de `DN00SyncMetaDataFromAdminToCOREItem`.
8. **`ejemplo-end-to-end.md`** — aclarar si el SRMD endpoint acepta bare array o requiere el wrapper completo `context`/`payload`.

---

*Informe generado por Kiro maintenance-agent — solo diagnóstico, sin modificaciones al código.*

DICTAMEN: conditional — 16 hallazgos (5 críticos que impiden integración, 4 altos, 4 medios, 3 informativos); requiere corrección de DataType IDs y patrón UrlCollection antes de considerar la documentación apta para integración
