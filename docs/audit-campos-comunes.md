# Auditoría: campos comunes en `data-retrieve/data/`

> **Generado:** auditoria manual sobre los ficheros `.md` (idioma base) de `data/`  
> **Referencia:** `campos-comunes.md` — campos heredados de `DN00DENADataExchangedObjectBase`  
> **Alcance:** `notificacion`, `pago`, `expediente`, `cita`, `matricula`, `registro-persona`, `beneficio-economico`, `credencial-persona`, `libro-registro`, `persona`, `datos-medicos`, `registro-oficial`, `servicio-administrativo`, `unidad-organica`

---

## Campos comunes de referencia

| Campo | Oblig. | Descripción |
|-------|:------:|-------------|
| `oid` | ✅ | Identificador técnico único |
| `id` | ✅ | Identificador de negocio |
| `lastChangedAt` | ✅ | Última modificación |
| `urls` | ❌ | URLs de acceso (opcional, muy habitual) |
| `originAdmin` | ❌ | Administración de origen (DENA lo completa) |
| `aboutPerson` | ❌ | Referencia a la persona (DENA lo completa) |

---

## Leyenda

| Símbolo | Significado |
|---------|-------------|
| ✅ | Presente y con badge `común` |
| ⚠️ | Presente pero sin badge `común` |
| ❌ | Ausente |
| N/A | No aplica (tipo no identificado o sub-objeto) |
| — | Sin valor evaluable |

---

## Resumen ejecutivo

| Tipo de dato | `oid` tabla | `id` tabla | `lastChangedAt` tabla | `urls` tabla | `originAdmin` tabla | `aboutPerson` tabla | `urls` ejemplo | `originAdmin` ejemplo | `aboutPerson` ejemplo |
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| `notificacion` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| `pago` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ⚠️ (1/3) | ❌ | ❌ |
| `expediente` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ | ❌ | ❌ |
| `cita` | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ⚠️ (1/2) | ❌ | ❌ |
| `registro-persona` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `beneficio-economico` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `matricula` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `credencial-persona` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `libro-registro` | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `persona` | N/A | N/A | ⚠️ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `datos-medicos` | N/A | N/A | ⚠️ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| `registro-oficial` | — | — | — | — | — | — | — | — | — |
| `servicio-administrativo` | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |
| `unidad-organica` | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A | N/A |

---

## Resultados detallados por tipo de dato

---

### 1. `notificacion.md` — `administrativeNotice`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ✅ | ✅ | Correcto |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (2: notificación pendiente, notificación leída)

| Campo común | Ejemplo 1 (pendiente) | Ejemplo 2 (leída) | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | |
| `id` | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | |
| `urls` | ✅ | ✅ | Array con 1 entrada en ambos |
| `originAdmin` | ❌ | ❌ | Ausente (opcional, DENA lo completa) |
| `aboutPerson` | ❌ | ❌ | Ausente (opcional, DENA lo completa) |

**Gaps:** `originAdmin` y `aboutPerson` no documentados en tabla ni en ejemplos.

---

### 2. `pago.md` — `payment` / `directDebitPayment`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅ (vía `DN00PaymentDataExchangedBase`)

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ✅ | ✅ | Correcto |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (3: pago único pendiente, pago único completado, domiciliación)

| Campo común | Pago único (pendiente) | Pago único (completado) | Domiciliación | Observaciones |
|---|:---:|:---:|:---:|---|
| `oid` | ✅ | ✅ | ✅ | |
| `id` | ✅ | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | ✅ | |
| `urls` | ✅ | ✅ | ❌ | **Falta `urls` en el ejemplo de domiciliación** |
| `originAdmin` | ❌ | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | ❌ | Ausente en todos |

**Gaps:** `originAdmin` y `aboutPerson` no documentados. `urls` ausente en el ejemplo de domiciliación (el campo está en la tabla pero no aparece en ese ejemplo).

---

### 3. `expediente.md` — `administrativeServiceProcedureRecord`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ✅ | ✅ | Correcto |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplo JSON (1)

| Campo común | Ejemplo | Observaciones |
|---|:---:|---|
| `oid` | ✅ | |
| `id` | ✅ | |
| `lastChangedAt` | ✅ | |
| `urls` | ✅ | Array con 2 entradas (ES + EU). El más completo de todos |
| `originAdmin` | ❌ | Ausente (opcional, DENA lo completa) |
| `aboutPerson` | ❌ | Ausente (opcional, DENA lo completa) |

**Gaps:** `originAdmin` y `aboutPerson` no documentados.

---

### 4. `cita.md` — `scheduleItem`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ✅ | ✅ | Correcto |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (2: cita con expediente, hito sin URL)

| Campo común | Cita principal | Hito (dur=0) | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | |
| `id` | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | |
| `urls` | ✅ | ❌ | **Falta `urls` en el ejemplo de hito** — considerando que las citas hito pueden no tener URL, sería razonable añadir un comentario explicitando por qué se omite |
| `originAdmin` | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | Ausente en todos |

**Gaps:** `originAdmin` y `aboutPerson` no documentados. `urls` ausente en el ejemplo de hito sin aclaración.

---

### 5. `registro-persona.md` — `administrativePersonRegister` y variantes

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ❌ | — | **Falta.** No aparece en ninguna fila de la tabla |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (3: PEV Vivienda, Revisión PEV, Últimas Voluntades)

| Campo común | PEV Vivienda | Revisión PEV | Últimas Voluntades | Observaciones |
|---|:---:|:---:|:---:|---|
| `oid` | ✅ | ✅ | ✅ | |
| `id` | ✅ | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | ✅ | |
| `urls` | ❌ | ❌ | ❌ | **Falta en todos los ejemplos** |
| `originAdmin` | ❌ | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | ❌ | Ausente en todos |

**Gaps:** `urls`, `originAdmin` y `aboutPerson` no documentados en tabla ni en ningún ejemplo. Este tipo tiene el mayor número de campos comunes faltantes en tabla.

---

### 6. `beneficio-economico.md` — `economicBenefit`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ❌ | — | **Falta.** No aparece en la tabla |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (2: concedida, pagada)

| Campo común | Concedida | Pagada | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | |
| `id` | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | |
| `urls` | ❌ | ❌ | **Falta en ambos ejemplos** |
| `originAdmin` | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | Ausente en todos |

**Gaps:** `urls`, `originAdmin` y `aboutPerson` no documentados en tabla ni en ejemplos.

---

### 7. `matricula.md` — `personEnrollment` / `personAcademicEnrollment`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅ (vía `DN00PersonEnrollmentBase`)

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ❌ | — | **Falta.** No aparece en la tabla |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (2: matrícula base, matrícula académica)

| Campo común | Matrícula base | Matrícula académica | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | |
| `id` | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | |
| `urls` | ❌ | ❌ | **Falta en ambos ejemplos** |
| `originAdmin` | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | Ausente en todos |

**Gaps:** `urls`, `originAdmin` y `aboutPerson` no documentados en tabla ni en ejemplos.

---

### 8. `credencial-persona.md` — `personCredential` / `personCredentialForBasqueLanguageDegree`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ❌ | — | **Falta.** No aparece en la tabla |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (2: credencial base, título de euskera)

| Campo común | Credencial base | Título euskera | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | |
| `id` | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | |
| `urls` | ❌ | ❌ | **Falta en ambos ejemplos** |
| `originAdmin` | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | Ausente en todos |

**Gaps:** `urls`, `originAdmin` y `aboutPerson` no documentados en tabla ni en ejemplos.

---

### 9. `libro-registro.md` — `administrativeLedger`

**Herencia:** `DN00DENADataExchangedObjectBase` ✅

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | Correcto |
| `id` | ✅ | ✅ | Correcto |
| `lastChangedAt` | ✅ | ✅ | Correcto |
| `urls` | ❌ | — | **Falta.** No aparece en la tabla |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplos JSON (2: asiento entrada, asiento salida)

| Campo común | Entrada | Salida | Observaciones |
|---|:---:|:---:|---|
| `oid` | ✅ | ✅ | |
| `id` | ✅ | ✅ | |
| `lastChangedAt` | ✅ | ✅ | |
| `urls` | ❌ | ❌ | **Falta en ambos ejemplos** |
| `originAdmin` | ❌ | ❌ | Ausente en todos |
| `aboutPerson` | ❌ | ❌ | Ausente en todos |

**Gaps:** `urls`, `originAdmin` y `aboutPerson` no documentados en tabla ni en ejemplos.

---

### 10. `persona.md` — `personData`

**Herencia:** `DN00DENADataExchangedNOTIdentifiedObject` ⚠️  
**Nota especial:** Este tipo extiende `NOT_IDENTIFIED`, por lo que `oid` e `id` **no existen** en este objeto. Esto está correctamente documentado en la tabla (marcados como `~~oid~~` y `~~id~~`). Sin embargo, `lastChangedAt`, `urls`, `originAdmin` y `aboutPerson` sí existen en la jerarquía y la propia documentación los menciona en las notas, pero no los recoge completamente en la tabla.

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | N/A | — | Correcto: documentado como "No existe" (~~`oid`~~) |
| `id` | N/A | — | Correcto: documentado como "No existe" (~~`id`~~) |
| `lastChangedAt` | ⚠️ | ❌ | Presente en la tabla pero **sin badge `común`** |
| `urls` | ❌ | — | **Falta.** El texto de la nota sí lo menciona ("SÍ tiene `lastChangedAt`, `urls`, `originAdmin` y `aboutPerson`") pero no hay fila en la tabla |
| `originAdmin` | ❌ | — | **Falta.** Mencionado en nota pero sin fila en tabla |
| `aboutPerson` | ❌ | — | **Falta.** Mencionado en nota pero sin fila en tabla |

#### Ejemplo JSON (1)

| Campo común | Ejemplo | Observaciones |
|---|:---:|---|
| `oid` | N/A | Correcto que no aparezca |
| `id` | N/A | Correcto que no aparezca |
| `lastChangedAt` | ✅ | Presente |
| `urls` | ❌ | **Falta en el ejemplo** |
| `originAdmin` | ❌ | Ausente |
| `aboutPerson` | ❌ | Ausente |

**Gaps:** `lastChangedAt` sin badge `común` en tabla. `urls`, `originAdmin` y `aboutPerson` mencionados en notas pero sin fila en tabla ni aparición en el ejemplo JSON.

---

### 11. `datos-medicos.md` — `personMedicalData`

**Herencia:** `DN00DENADataExchangedNOTIdentifiedObject` ⚠️ (vía `DN00PersonData`)  
**Nota especial:** Mismo caso que `persona.md`. `oid` e `id` no existen. `lastChangedAt`, `urls`, `originAdmin` y `aboutPerson` sí existen en la jerarquía.

#### Tabla de campos

| Campo común | En tabla | Badge `común` | Observaciones |
|---|:---:|:---:|---|
| `oid` | N/A | — | Correcto: no existe, mencionado en texto |
| `id` | N/A | — | Correcto: no existe, mencionado en texto |
| `lastChangedAt` | ⚠️ | ❌ | Presente en la tabla pero **sin badge `común`** |
| `urls` | ❌ | — | **Falta.** No aparece en la tabla |
| `originAdmin` | ❌ | — | **Falta.** No aparece en la tabla |
| `aboutPerson` | ❌ | — | **Falta.** No aparece en la tabla |

#### Ejemplo JSON (1)

| Campo común | Ejemplo | Observaciones |
|---|:---:|---|
| `oid` | N/A | Correcto que no aparezca |
| `id` | N/A | Correcto que no aparezca |
| `lastChangedAt` | ✅ | Presente |
| `urls` | ❌ | **Falta en el ejemplo** |
| `originAdmin` | ❌ | Ausente |
| `aboutPerson` | ❌ | Ausente |

**Gaps:** `lastChangedAt` sin badge `común`. `urls`, `originAdmin` y `aboutPerson` no documentados en tabla ni en ejemplo.

---

## Tipos excluidos del análisis de campos comunes

### `registro-oficial.md` — OBSOLETO

El fichero documenta el tipo `administrativeOfficialRegisterRecord`, que está **obsoleto** desde v0.5.20: no existe en `DN00DataTypeEnum` ni tiene implementación Java. El propio fichero lo indica con una advertencia de peligro. No contiene tabla de campos ni ejemplo JSON, por lo que no hay nada que auditar. Los nuevos desarrollos deben usar `administrativeLedger` (ver `libro-registro.md`).

### `servicio-administrativo.md` — Sub-objeto

`DN00AdmistrativeService` y `DN00AdministrativeServiceProcedure` son **sub-objetos embebidos dentro de `expediente`**, no tipos de dato raíz del data-retrieve. No extienden `DN00DENADataExchangedObjectBase` directamente y no tienen sus propios `oid` / `id` / `lastChangedAt` como campos de primer nivel. Los campos comunes no aplican a este fichero.

### `unidad-organica.md` — Sub-objeto

`DN00OrgUnitReference` / `DN00OrgUnitReferenceWithRole` son **sub-objetos embebidos** dentro de servicios y procedimientos. No son tipos de dato raíz del data-retrieve. Los campos comunes no aplican a este fichero.

---

## Hallazgos globales

### H1 — `originAdmin` y `aboutPerson` ausentes en **todos** los tipos

Ningún fichero documenta `originAdmin` ni `aboutPerson` en la tabla de campos ni en los ejemplos JSON. Son campos opcionales que DENA completa, pero dado que aparecen en `campos-comunes.md` como parte del contrato base, **todos los tipos deberían tener una fila en su tabla** indicando su existencia aunque con badge `común` y marcados como opcionales.

**Afectados:** todos los tipos que heredan de `DN00DENADataExchangedObjectBase` (11 tipos) y los dos `NOT_IDENTIFIED` (persona, datos-medicos).

### H2 — `urls` ausente en tabla de 7 tipos

Los tipos `registro-persona`, `beneficio-economico`, `matricula`, `credencial-persona`, `libro-registro`, `persona` y `datos-medicos` no incluyen `urls` en su tabla de campos.

**Impacto:** Los integradores que lean únicamente esos ficheros no sabrán que el campo existe. Los 4 primeros tipos (`notificacion`, `pago`, `expediente`, `cita`) sí lo documentan correctamente.

### H3 — `urls` ausente en algunos ejemplos JSON de tipos que sí la documentan en tabla

Aunque `pago` y `cita` tienen `urls` en su tabla, en ciertos ejemplos concretos el campo no aparece:
- `pago.md`: ejemplo de domiciliación omite `urls`
- `cita.md`: ejemplo de hito (duración = 0) omite `urls`

Sería deseable incluir `urls` en esos ejemplos (aunque sea con valor `null` o array vacío) o añadir una nota explícita que justifique la omisión.

### H4 — `lastChangedAt` sin badge `común` en `persona.md` y `datos-medicos.md`

En estos dos ficheros `lastChangedAt` está presente en la tabla pero no lleva el badge `común`, a diferencia del resto de tipos. Dado que sí es un campo heredado de la clase base (aunque sea `NOT_IDENTIFIED`), debería marcarse como `común` para consistencia.

---

## Acciones recomendadas (priorizadas)

| Prioridad | Acción | Ficheros afectados |
|:---------:|--------|--------------------|
| 🔴 Alta | Añadir filas de `urls`, `originAdmin`, `aboutPerson` con badge `común` a las tablas | `registro-persona`, `beneficio-economico`, `matricula`, `credencial-persona`, `libro-registro` |
| 🔴 Alta | Añadir filas de `urls`, `originAdmin`, `aboutPerson` en tabla (sin badge, con nota de jerarquía diferente) | `persona`, `datos-medicos` |
| 🟡 Media | Añadir filas de `originAdmin` y `aboutPerson` con badge `común` a las tablas que ya tienen `urls` | `notificacion`, `pago`, `expediente`, `cita` |
| 🟡 Media | Incluir `urls` en el ejemplo JSON de domiciliación (`pago.md`) y en el ejemplo de hito (`cita.md`), o añadir nota que justifique la omisión | `pago`, `cita` |
| 🟡 Media | Añadir `urls` en al menos un ejemplo JSON por tipo | `registro-persona`, `beneficio-economico`, `matricula`, `credencial-persona`, `libro-registro` |
| 🟢 Baja | Añadir badge `común` a `lastChangedAt` en tabla | `persona`, `datos-medicos` |
| 🟢 Baja | Considerar añadir `originAdmin` y `aboutPerson` al menos en un ejemplo JSON (comentados o con `null`) | todos los tipos estándar |

---

<!-- DENA-DOC-FOOTER -->
---
<sub>Auditoría generada con Kiro · DENA Docs v{{ dena.version }}</sub>
