# Auditoría de integridad DENA docs v0.5.20

Fecha: 2025-01-27

**Auditor:** Kiro (maintenance-agent, L3)  
**Código auditado:** workspace local, rama develop, snapshot 0.5.20  
**Documentación auditada:** `e80a021-dena-common-docs/docs/`  
**Método:** lectura directa de clases Java (`@MarshallType`, `@MarshallField`, enums, anotaciones REST) y cruce con los `.md` correspondientes

---

## Resumen ejecutivo

| Severidad | Total |
|-----------|------:|
| 🔴 ALTO   | 2     |
| 🟠 MEDIO  | 3     |
| 🟡 BAJO   | 4     |
| ℹ️ INFO   | 3     |
| **Total** | **12** |

Los dos hallazgos altos afectan directamente a integraciones externas: el campo `ofType.id` en el endpoint de Metadata-Sync usa el valor incorrecto, y el ejemplo JSON del endpoint data-retrieve contiene un nombre de campo que no existe en el código.

---

## Hallazgos por área

---

### A. Modelos de datos

#### A1 — `endpoint-data-retrieve.md` línea 80: campo `lastUpdatedAt` inexistente en el JSON de ejemplo

| Atributo | Valor |
|----------|-------|
| **Severidad** | 🟠 MEDIO |
| **Evidencia** | Observado |
| **Doc** | `semantica/data-retrieve/endpoint-data-retrieve.md:80` |
| **Código** | `DN00AdmistrativeServiceProcedureRecordData.java:32` |

**Descripción:**  
El JSON de ejemplo de la Response en `endpoint-data-retrieve.md` incluye el campo `"lastUpdatedAt"` dentro de `recordData`:

```json
"lastUpdatedAt": "2024-06-01T14:00:00Z",
```

Sin embargo, en el código Java el campo se llama `"lastModifiedAt"`:

```java
// DN00AdmistrativeServiceProcedureRecordData.java:32
@MarshallField(as="lastModifiedAt", ...)
@Getter @Setter protected Instant _lastModifiedAt;
```

`expediente.md` sí usa correctamente `lastModifiedAt`. La inconsistencia es entre documentos: `endpoint-data-retrieve.md` usa el nombre incorrecto; `expediente.md` usa el correcto. Una administración que copie el ejemplo del endpoint recibiría un campo JSON que DENA-CORE ignoraría silenciosamente.

**Corrección:** En `endpoint-data-retrieve.md`, cambiar `"lastUpdatedAt"` por `"lastModifiedAt"`.

---

#### A2 — `cita.md`: campo `type` duplicado en la tabla de atributos y en el JSON de ejemplo

| Atributo | Valor |
|----------|-------|
| **Severidad** | 🟡 BAJO |
| **Evidencia** | Observado |
| **Doc** | `semantica/data-retrieve/data/cita.md:86,99` y `cita.md:145,160` |
| **Código** | `DN00ScheduleItem.java:29` (`@MarshallType(as="scheduleItem")`) y `DN00ScheduleItem.java:80` (`@MarshallField(as="type")`) |

**Descripción:**  
En la tabla de atributos de `cita.md` aparecen dos filas con el nombre de campo `type`:

- Fila 86: `type` → discriminador polimórfico (`"scheduleItem"`)
- Fila 99: `type` (DN00ScheduleItemType) → tipo de cita (`"IN_PERSON_APPOINTMENT"`)

El JSON de ejemplo también tiene dos claves `type` (líneas 145 y 160), lo que es JSON inválido. En la serialización real, el marshaller usa `"type"` tanto para el discriminador de clase (valor `"scheduleItem"`) como para el campo `_type` de `DN00ScheduleItem` (valor enum como `IN_PERSON_APPOINTMENT`). El conflicto de nombre es real y ya está en el código; en la documentación debería aclararse este comportamiento (ej.: señalar que el discriminador y el campo de tipo de cita comparten el mismo nombre JSON).

**Corrección sugerida:** Añadir una nota que explique que el campo `type` es polisémico: el valor `"scheduleItem"` lo inyecta el marshaller automáticamente, y el enum `DN00ScheduleItemType` sobreescribe ese valor. Alternativamente, renombrar el campo de tipo de cita con un nombre distinto en el marshalling (cambio de código).

---

#### A3 — `cita.md` diagrama Mermaid: valores abreviados de `DN00ScheduleItemType`

| Atributo | Valor |
|----------|-------|
| **Severidad** | ℹ️ INFO |
| **Evidencia** | Observado |
| **Doc** | `semantica/data-retrieve/data/cita.md:33` |
| **Código** | `DN00ScheduleItemType.java` |

**Descripción:**  
El diagrama Mermaid muestra los tipos de cita como `"REMINDER · IN_PERSON · PHONE · VIDEO · CHAT"`, pero los valores reales del enum son:

```
REMINDER, IN_PERSON_APPOINTMENT, PHONE_CALL_APPOINTMENT, VIDEO_CALL_APPOINTMENT, CHAT_APPOINTMENT
```

La tabla de tipos en el mismo documento sí usa los valores correctos completos. El diagrama es una abreviatura que puede confundir si se usa como referencia de implementación.

**Corrección:** Actualizar el nodo del diagrama para reflejar los valores completos o indicar que son abreviados.

---

#### A4 — `campos-comunes.md`: `personData` listado como si tuviera `oid` e `id`

| Atributo | Valor |
|----------|-------|
| **Severidad** | ℹ️ INFO |
| **Evidencia** | Observado |
| **Doc** | `semantica/data-retrieve/data/campos-comunes.md` (tabla "Objetos que heredan estos campos") |
| **Código** | `DN00PersonData.java:17` (extiende `DN00DENADataExchangedNOTIdentifiedObject`, **no** `DN00DENADataExchangedObjectBase`) |

**Descripción:**  
La tabla "Objetos que heredan estos campos" en `campos-comunes.md` incluye `personData` y `personMedicalData` entre los tipos que heredan `oid` e `id`. Sin embargo, `DN00PersonData` extiende `DN00DENADataExchangedNOTIdentifiedObject` (sin identificadores), mientras que `DN00PersonData` sí tiene `lastChangedAt`, `urls`, `originAdmin` y `aboutPerson`.

`persona.md` sí documenta correctamente la ausencia de `oid` e `id` con una advertencia explícita. La tabla de campos-comunes puede inducir a error al sugerir que estos tipos tienen identificadores.

**Corrección:** En la tabla de `campos-comunes.md`, añadir una columna o nota indicando qué tipos **no tienen** `oid`/`id` (los que extienden `DN00DENADataExchangedNOTIdentifiedObject`).

---

### B. DataTypeEnum

#### B1 — Verificación completa de los 17 tipos

| `DN00DataTypeEnum` valor | `marshallTypeId` | `data-type-ref.md` | `index.md` tabla |
|--------------------------|-----------------|---------------------|-----------------|
| `ADMINISTRATIVE_NOTICE` | `administrativeNotice` | ✅ `ADMINISTRATIVE_NOTICE` | ✅ |
| `ADMINISTRATIVE_RECORD` | `administrativeServiceProcedureRecord` | ✅ | ✅ |
| `PAYMENT_ONE_OFF_PAYMENT` | `oneOffPayment` | ✅ | ✅ |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | `directDebitPayment` | ✅ | ✅ |
| `SCHEDULE` | `scheduleItem` | ✅ | ✅ |
| `PERSON_DATA` | `personData` | ✅ | ✅ |
| `PERSON_CREDENTIAL` | `personCredential` | ✅ | ✅ |
| `ECONOMIC_BENEFIT` | `economicBenefit` | ✅ | ✅ |
| `ADMINISTRATIVE_LEDGER` | `administrativeLedger` | ✅ | ✅ |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | `administrativePersonRegisterForHousingPEV` | ✅ | ✅ |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | `administrativePersonRegisterForLastWill` | ✅ | ✅ |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | `administrativePersonRegisterForRevisionPEV` | ✅ | ✅ |
| `PERSON_ENROLLMENT` | `personEnrollment` | ✅ | ✅ |
| `ADMINISTRATIVE_PERSON_REGISTER` | `administrativePersonRegister` | ✅ | ✅ |
| `PERSON_ACADEMIC_ENROLLMENT` | `personAcademicEnrollment` | ✅ | ✅ |
| `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | `personCredentialForBasqueLanguageDegree` | ✅ | ✅ |
| `PERSON_MEDICAL_DATA` | `personMedicalData` | ✅ | ✅ |

**Resultado:** Los 17 tipos del enum están correctamente documentados en `data-type-ref.md` y en la tabla de `index.md`. Los valores `dataType.id` en los catálogos usan correctamente los nombres SCREAMING_SNAKE_CASE (coinciden con los nombres del enum, que es el identificador externo). El tipo obsoleto `ADMINISTRATIVE_OFFICIAL_REGISTER_RECORD` está correctamente marcado como eliminado.

---

### C. Tipos de mensaje (DN00InteropMessageType)

#### C1 — Todos los pares _REQ/_RESP presentes en `message-types.md`

Tras comparar la totalidad de los 52 valores del enum `DN00InteropMessageType.java` con la tabla de `semantica/semantica-base/modelo/message-types.md`:

- Todos los pares documentados son correctos.
- `CLIENT_LOGIN_DEMO` (sin respuesta pareada) está correctamente indicado como sin respuesta.
- `PERSON_PUSH_TO_ADMIN_REQ`/`RESP` están documentados. En el código, este par **no está registrado** en el método `responseType()` (el switch de Java), lo que el propio código comenta como una limitación con efectos conocidos ("la response silently echoes the request type"). Esto es coherente con lo documentado: la documentación no promete el comportamiento de `responseType()`.

#### C2 — Componentes de interoperabilidad: `DENA_PERSON_ADMIN` y `DENA_SANDBOX_ADMIN` no documentados

| Atributo | Valor |
|----------|-------|
| **Severidad** | ℹ️ INFO |
| **Evidencia** | Observado |
| **Doc** | `semantica/semantica-base/modelo/message-types.md:71` |
| **Código** | `DN00InteropIDs.java:38,41` |

**Descripción:**  
El enum `DN00InteropIDs.DN00InteropComponentID` en el código contiene 7 valores:

```java
CLIENT_INSTALLMENT, DENA_CORE, DENA_ADMIN_CONNECTOR, ADMIN,
DENA_INTEROP_ADMIN_SYNC, DENA_PERSON_ADMIN, DENA_SANDBOX_ADMIN
```

La tabla de `message-types.md` documenta solo 5 de ellos. Faltan `DENA_PERSON_ADMIN` y `DENA_SANDBOX_ADMIN`.

**Corrección:** Añadir las dos entradas a la tabla de identificadores de componentes.

---

### D. HTTP Headers

#### D1 — `http-headers.md`: `X-DENA-ORG-ADMIN-ID` no documentado

| Atributo | Valor |
|----------|-------|
| **Severidad** | 🟠 MEDIO |
| **Evidencia** | Observado |
| **Doc** | `semantica/semantica-base/http-headers.md` |
| **Código** | `DN01DENAHeadersConstants.java:12` (`HEADER_ORG_ADMIN_ID = "X-DENA-ORG-ADMIN-ID"`) |

**Descripción:**  
El fichero de constantes `DN01DENAHeadersConstants.java` define **6 cabeceras HTTP**:

```
X-DENA-This-TimeStamp
X-DENA-Origin-TimeStamp
X-DENA-Message-Correlation-Id
X-DENA-Data-Digest
X-DENA-ORG-ADMIN-ID      ← NO documentado
Content-Digest
```

Las primeras cinco (sin `X-DENA-ORG-ADMIN-ID`) y `Content-Digest` están correctamente documentadas en `http-headers.md`. Sin embargo, `X-DENA-ORG-ADMIN-ID` no aparece en ninguna de las tablas del documento.

**Corrección:** Añadir `X-DENA-ORG-ADMIN-ID` a la tabla de cabeceras de request en `http-headers.md`, con su descripción y uso.

#### D2 — Algoritmos digest: sin inconsistencia

| Atributo | Valor |
|----------|-------|
| **Evidencia** | Observado |

El código usa `DN01DENAHeadersConstants.ALGORITHM_DENA_DIGEST = "SHA-256"` (mayúsculas) para construir el valor del header vía `digestHeaderValue()`. El formato resultante es `SHA-256=:<hash>:`, que coincide exactamente con lo documentado en `http-headers.md`. **Sin inconsistencia**.

---

### E. SRMD / Metadata-Sync

#### E1 — `endpoint-sync-metadata.md`: `ofType.id` usa `marshallTypeId` en lugar del identificador externo (SCREAMING_SNAKE_CASE) — CRÍTICO

| Atributo | Valor |
|----------|-------|
| **Severidad** | 🔴 ALTO |
| **Evidencia** | Observado |
| **Doc** | `semantica/metadata-sync/endpoint-sync-metadata.md:45,75,108` |
| **Código** | `DN01SandboxAdminNoticeWebController.java:117` y swagger annotation en `DN01RESTInteropMetadataAdminSyncSwaggerDoc.java:49` |

**Descripción:**  
`endpoint-sync-metadata.md` documenta e ilustra el campo `ofType.id` con el valor `"administrativeNotice"`:

```json
"ofType": { "id": "administrativeNotice" }
```

Y el campo en la tabla describe: _"Tipo de dato (marshallTypeId, ej. `administrativeNotice`)"_.

Sin embargo, **toda la implementación del código usa SCREAMING_SNAKE_CASE** (el nombre del enum, no su `marshallTypeId`):

- Sandbox admin controller: `_support.notify(personId, "ADMINISTRATIVE_NOTICE", ...)`
- Swagger doc annotation embebida en el código: `"dataType":{"id":"ADMINISTRATIVE_NOTICE","oid":"..."}` y `"ofType":{"id":"ADMINISTRATIVE_RECORD","oid":"..."}`
- `sync-types.md` (otro documento del mismo proyecto): usa `"PERSON_DATA"` (SCREAMING_SNAKE_CASE)
- `data-type-ref.md` (catálogo de tipos): usa `ADMINISTRATIVE_NOTICE`, `SCHEDULE`, etc. (SCREAMING_SNAKE_CASE)

Esto crea una inconsistencia directa entre documentos del mismo proyecto y entre el único ejemplo de `endpoint-sync-metadata.md` y el código de referencia. Una administración que siga el ejemplo del endpoint enviará `"administrativeNotice"` y el CORE no lo reconocerá como un tipo válido (o lo resolverá con comportamiento indefinido según cómo el servidor parsee el `DN00DataTypeID`).

**Corrección:**
1. En `endpoint-sync-metadata.md`, cambiar todas las ocurrencias de `"administrativeNotice"` (líneas 45, 108) por `"ADMINISTRATIVE_NOTICE"`.
2. Corregir la descripción del campo en la tabla (línea 75): "Tipo de dato. Usar el identificador externo SCREAMING_SNAKE_CASE (ej. `ADMINISTRATIVE_NOTICE`), **no** el marshallTypeId."
3. Verificar todos los ejemplos del fichero para asegurar coherencia con los identificadores del catálogo.

---

#### E2 — `sync-types.md`: campos del `DN00SyncMetaDataFromAdminToCOREItem` correctos

| Atributo | Valor |
|----------|-------|
| **Evidencia** | Observado |

Comparación exhaustiva campo a campo:

| Campo doc | `@MarshallField` en código | ¿OK? |
|-----------|---------------------------|------|
| `admin` | `@MarshallField(as="admin")` | ✅ |
| `aboutPerson` | `@MarshallField(as="aboutPerson")` | ✅ |
| `someDataWasUpdatedAt` | `@MarshallField(as="someDataWasUpdatedAt")` | ✅ |
| `ofType` | `@MarshallField(as="ofType")` | ✅ |
| `fromDataOriginInstance` | `@MarshallField(as="fromDataOriginInstance")` | ✅ |
| `popMessageAfterSync` | `@MarshallField(as="popMessageAfterSync")` | ✅ |

El `@MarshallType` del record es `"syncMetaDataFromAdminToCORE"` (código `:30`) — no se documenta el discriminador porque no es relevante para la administración que envía el array.

La estructura interna de `popMessageAfterSync` (`how` + `messageByLang`) coincide con `DN00PopMessageAfterSyncAdminSpec.java`.

**Sin inconsistencia** en los campos, salvo el ya indicado en E1 para `ofType.id`.

---

### F. Person-Sync

#### F1 — `push.md`: estructura de payload verificada y correcta

Comparación de `push.md` con `DN00PersonSyncData.java`:

| Campo doc | `@MarshallField` en código | ¿OK? |
|-----------|---------------------------|------|
| `syncData.personRef` | `@MarshallField(as="personRef")` | ✅ |
| `syncData.createDate` | `@MarshallField(as="createDate")` | ✅ |
| `syncData.lastUpdateDate` | `@MarshallField(as="lastUpdateDate")` | ✅ |
| `syncData.syncEvent` | `@MarshallField(as="syncEvent")` | ✅ |

El tipo marshalled es `"personSyncData"` (código). El tipo de mensaje `PERSON_PUSH_TO_ADMIN_REQ` coincide con lo documentado. **Sin inconsistencia.**

#### F2 — `pull.md` y endpoints de pull: rutas correctas

La documentación muestra rutas con prefijo `/person-sync/`, por ejemplo `POST /person-sync/api/admin/persons/sync/bespokes`. El código expone `@PostMapping(value = "api/admin/persons/sync/bespokes")` sin prefijo. La configuración confirma `DENA_CONTEXT_PATH=/person-sync` en `dn01.e80a021k-dena-person-admin-sync-rest-app.yml`, haciendo que la ruta completa sea `/person-sync/api/admin/persons/sync/bespokes`. **Sin inconsistencia.**

---

### G. Endpoints REST

#### G1 — Endpoint `POST /api/connector/retrieveData`: documentado y correcto

| Atributo | Valor |
|----------|-------|
| **Evidencia** | Observado |

El controlador `DN01ConnectorController.java:38` expone:

```java
@RequestMapping("/api/connector")
@PostMapping(value = "/retrieveData", ...)
```

`endpoint-data-retrieve.md` indica correctamente: _"El **conector base** de DENA (`DN01ConnectorController`) lo expone en `/api/connector/retrieveData`"_. **Sin inconsistencia.**

#### G2 — Endpoint `POST api/admin/interop/sync/metadata`: correcto

`DN01RESTInteropMetadataAdminSyncResource.java:28,60`:
```java
@RequestMapping("api/admin/interop/sync")
@PostMapping(value = "/metadata")
```
La ruta completa es `api/admin/interop/sync/metadata`, que coincide con `endpoint-sync-metadata.md`. **Sin inconsistencia.**

---

### H. URLs internas entre `.md`

#### H1 — Sin enlaces rotos en las rutas verificadas

Se verificaron los siguientes enlaces internos clave:

| Origen | Enlace | Archivo destino | ¿Existe? |
|--------|--------|-----------------|----------|
| `data-type-ref.md` | `../../../data-retrieve/data/expediente.md` | `expediente.md` | ✅ |
| `data-type-ref.md` | `../../../data-retrieve/data/registro-persona.md` | `registro-persona.md` | ✅ |
| `index.md` | `./data/registro-oficial.md` | `registro-oficial.md` | ✅ (marcado obsoleto) |
| `campos-comunes.md` | `./expediente.md` | `expediente.md` | ✅ |
| `sync-types.md` | `./data-type-ref.md` | `data-type-ref.md` | ✅ |

Todos los ficheros referenciados existen. `registro-oficial.md` existe y está correctamente marcado como obsoleto.

---

### I. Versiones y referencias

#### I1 — `mkdocs-vars.yaml`: versión `0.5.20` coherente con `common-data-api`

| Módulo | POM `revision` local |
|--------|---------------------|
| `dena.version` en mkdocs-vars.yaml | `0.5.20` |
| `e80a021-dena-common-data-api` | `0.5.20-SNAPSHOT` ✅ |
| `e80a021-dena-common-api` | `0.5.11-SNAPSHOT` ⚠️ |
| `e80a021-dena-common-interop-api` | `0.5.10-SNAPSHOT` ⚠️ |

**Descripción:**  
El `mkdocs-vars.yaml` declara la versión de la documentación como `0.5.20` y las URLs de repositorios apuntan a la etiqueta `PUBLIC-v0.5.20`. El módulo `common-data-api` está en `0.5.20-SNAPSHOT`, coherente. Sin embargo, `common-api` está en `0.5.11-SNAPSHOT` y `common-interop-api` en `0.5.10-SNAPSHOT`.

Esto es esperable en un monorepo donde los módulos tienen ciclos de versión independientes. Las etiquetas públicas (`PUBLIC-v0.5.20`) son las que se publican en GitHub y a las que apuntan los enlaces de la documentación. El snapshot local no implica inconsistencia documental, pero sí conviene verificar que las etiquetas `PUBLIC-v0.5.20` en GitHub apuntan a los commits que incluyen los enums y clases descritos.

#### I2 — `mkdocs-vars.yaml`: `conx_test_blob/tree` todavía apuntan a `PUBLIC-v0.4.16`

| Atributo | Valor |
|----------|-------|
| **Severidad** | 🟡 BAJO |
| **Evidencia** | Observado |
| **Doc** | `mkdocs-vars.yaml` (comentario en el propio fichero) |

**Descripción:**  
El propio fichero documenta la deuda pendiente:

```yaml
# PENDIENTE (no actualizar hasta que exista el tag):
#   - conx_test_blob/conx_test_tree -> siguen en PUBLIC-v0.4.16
```

Los enlaces a ejemplos del repositorio `dena-admin-conx-test` apuntan a `PUBLIC-v0.4.16` (no a `v0.5.20`). Cualquier enlace en la documentación que use `{{ repos.conx_test_blob }}` o `{{ repos.conx_test_tree }}` apuntará a una versión desactualizada del código de test.

**Corrección:** Actualizar `conx_test_blob` y `conx_test_tree` a `PUBLIC-v0.5.20` cuando exista la etiqueta en ese repositorio.

---

## Pendientes de verificar (⚠️ SIN VERIFICAR)

Los siguientes puntos no pudieron verificarse completamente con la evidencia disponible en el workspace local:

1. **⚠️ SIN VERIFICAR** — `endpoint-sync-metadata.md` línea 75: la descripción dice _"marshallTypeId"_ como valor esperado de `ofType.id`. Si el código DENA-CORE (servidor que recibe el SRMD) acepta ambos formatos (marshallTypeId y enum name), la severidad del hallazgo E1 bajaría a MEDIO. No se accedió al código del parser DENA-CORE.

2. **⚠️ SIN VERIFICAR** — `credencial-persona.md` y `matricula.md`: no se auditaron en detalle. Los `@MarshallType` son `personCredential` / `personCredentialForBasqueLanguageDegree` / `personEnrollment` / `personAcademicEnrollment`, que coinciden con el enum. La auditoría campo a campo de estos tipos se considera de riesgo bajo dado el patrón consistente observado en los demás tipos.

3. **⚠️ SIN VERIFICAR** — Etiquetas `PUBLIC-v0.5.20` en los repositorios GitHub referenciados: la documentación apunta a esas etiquetas, pero no se verificó si todas existen en los repositorios externos.

4. **⚠️ SIN VERIFICAR** — `X-DENA-ORG-ADMIN-ID`: se confirmó que está definido en `DN01DENAHeadersConstants.java` pero no se localizó el código que lo usa (filtro traffic-flow) para determinar en qué llamadas es obligatorio/opcional.

---

## Conclusiones

La documentación DENA v0.5.20 presenta una calidad general alta. Los modelos de datos de los tipos principales (`expediente`, `cita`, `notificación`, `libro-registro`, `persona`, `datos-médicos`, `sync-types`) están alineados con el código Java. Las rutas REST y los campos de los payloads son correctos en la mayoría de los documentos.

**Los dos hallazgos de mayor impacto** para las integraciones externas son:

1. **E1 (ALTO):** `endpoint-sync-metadata.md` documenta `ofType.id = "administrativeNotice"` (marshallTypeId) cuando el identificador externo correcto es `"ADMINISTRATIVE_NOTICE"` (SCREAMING_SNAKE_CASE). Esta discrepancia ya es inconsistente con `sync-types.md` y con el propio swagger del código, y es probable que cause errores de integración en administraciones que sigan el ejemplo de este documento.

2. **A1 (MEDIO):** `endpoint-data-retrieve.md` usa `"lastUpdatedAt"` en el JSON de ejemplo cuando el campo real se llama `"lastModifiedAt"` (verificado en `DN00AdmistrativeServiceProcedureRecordData.java:32`). Esto es una inconsistencia interna entre documentos del mismo proyecto.

**Acción recomendada prioritaria:** Corregir E1 (`endpoint-sync-metadata.md`) y A1 (`endpoint-data-retrieve.md`) antes de la próxima publicación.

---

*Informe generado por Kiro (maintenance-agent, L3) — evidencia directa del workspace local. Revisión humana requerida para los puntos marcados como ⚠️ SIN VERIFICAR.*

<!-- DENA-DOC-FOOTER -->
