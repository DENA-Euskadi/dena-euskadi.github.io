# Validaciones y Reglas de Formato

## Descripción

Este documento describe las reglas de validación, formatos esperados y restricciones que deben cumplir los datos devueltos por la administración en el endpoint DATA-RETRIEVE.

---

## Identificadores

### OID (Identificador técnico)

| Regla | Detalle |
|-------|---------|
| Tipo | `String` |
| Obligatorio | ✅ Sí |
| Formato | Alfanumérico libre, sin espacios |
| Longitud máxima | 255 caracteres |
| Unicidad | Único por tipo de objeto dentro de la administración |
| Ejemplo | `EXP-OID-001`, `NOT-OID-456`, `PAY-OID-789` |

### ID (Identificador de negocio)

| Regla | Detalle |
|-------|---------|
| Tipo | `String` |
| Obligatorio | ✅ Sí |
| Formato | Alfanumérico, puede incluir guiones y puntos |
| Longitud máxima | 255 caracteres |
| Unicidad | Único por tipo de objeto dentro de la administración |
| Ejemplo | `EXP-2024-00123`, `NOT-2024-00456` |

---

## Fechas y horas

### Formato ISO 8601 (campos `Instant`)

Campos como `createdAt`, `lastUpdatedAt`, `issuedAt`, `readedAt`, `registeredAt`.

| Regla | Detalle |
|-------|---------|
| Formato | ISO 8601 con zona horaria |
| Zona horaria | Se recomienda UTC (`Z`), pero se acepta offset (`+02:00`) |
| Ejemplos válidos | `2024-03-15T10:30:00Z`, `2024-03-15T12:30:00+02:00` |
| Ejemplos inválidos | `2024-03-15`, `15/03/2024 10:30` |

### Formato fecha (campos `LocalDate`)

Campos como `dueDate`, `surchargedAt`, `paidAt`, `setAt`, `expiresAt`, `nextChargeAt`.

| Regla | Detalle |
|-------|---------|
| Formato | `YYYY-MM-DD` |
| Ejemplos válidos | `2024-06-30`, `2024-07-15` |
| Ejemplos inválidos | `30/06/2024`, `2024-6-30` |

---

## Textos multiidioma (`LanguageTexts`)

| Regla | Detalle |
|-------|---------|
| Tipo | Objeto JSON con claves de idioma |
| Idiomas obligatorios | `SPANISH` y `BASQUE` (mínimo) |
| Idiomas opcionales | `ENGLISH` |
| Longitud máxima por texto | Sin límite estricto, pero se recomienda < 500 caracteres |
| Valores vacíos | No se permiten cadenas vacías (`""`) ni solo espacios |

```json
{
  "SPANISH": "Texto en castellano",
  "BASQUE": "Testua euskaraz"
}
```

### Claves de idioma válidas

| Clave | Idioma |
|-------|--------|
| `SPANISH` | Castellano |
| `BASQUE` | Euskera |
| `ENGLISH` | Inglés |

---

## URLs

| Regla | Detalle |
|-------|---------|
| Protocolo | Solo `https://` (se acepta `http://` en entornos de prueba) |
| Formato | URL válida según RFC 3986 |
| Accesibilidad | La URL debe ser accesible para la persona ciudadana |

---

## Importes monetarios

| Regla | Detalle |
|-------|---------|
| Tipo | `Number` (punto decimal) |
| Moneda | Siempre euros (€) |
| Precisión | Máximo 2 decimales |
| Ejemplos válidos | `45.50`, `120.00`, `0.99` |
| Ejemplos inválidos | `45,50` (coma), `45.505` (3 decimales) |

---

## Reglas de coherencia

### Expediente

| Regla | Detalle |
|-------|---------|
| `createdAt` ≤ `lastUpdatedAt` | La fecha de creación no puede ser posterior a la última actualización |
| `state.stateCode` = `CLOSED` | Si está cerrado, `lastUpdatedAt` debe reflejar la fecha de cierre |

### Notificación

| Regla | Detalle |
|-------|---------|
| `readedAt` = `null` si `state` = `PENDING_TO_BE_READED_BY_DESTINATION` | No puede tener fecha de lectura si está pendiente |
| `readedAt` ≠ `null` si `state` = `ACKNOWLEDGED_BY_DESTINATION` o `REJECTED_BY_DESTINATION` | Debe tener fecha de lectura si fue leída/rechazada |
| `issuedAt` ≤ `readedAt` | La fecha de emisión no puede ser posterior a la de lectura |

### Pago único

| Regla | Detalle |
|-------|---------|
| `data.forStatus` = `COMPLETED` → `paymentDates.paidAt` ≠ `null` | Si está completado, debe tener fecha de pago |
| `data.forStatus` = `PENDING` → `paymentDates.paidAt` = `null` | Si está pendiente, no debe tener fecha de pago |
| `amountInEuro` > 0 | El importe debe ser positivo |
| `amountInEuroIfSurcharged` ≥ `amountInEuro` | El importe con recargo no puede ser menor que el normal |
| `paymentDates.dueDate` ≤ `paymentDates.surchargedAt` | El vencimiento es anterior al inicio del recargo |

### Domiciliación

| Regla | Detalle |
|-------|---------|
| `directDebitData.expiresAt` = `null` o ≥ fecha actual | Si tiene expiración, no debe estar ya expirada (salvo históricos) |
| `nextChargeAmountInEuro` > 0 | El importe del próximo cargo debe ser positivo |

### Cita

| Regla | Detalle |
|-------|---------|
| `monthOfYear` entre 1 y 12 | Mes válido |
| `dayOfMonth` entre 1 y 31 | Día válido para el mes |
| `hourOfDay` entre 0 y 23 | Hora válida |
| `minuteOfHour` entre 0 y 59 | Minuto válido |
| `durationMinutes` ≥ 0 | Duración no negativa (0 = hito) |

---

## Campos obligatorios por tipo de objeto

### Expediente (`administrativeServiceProcedureRecord`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `service` | ✅ |
| `procedure` | ✅ |
| `createdAt` | ✅ |
| `state` | ✅ |
| `lastUpdatedAt` | ❌ |
| `descriptionByLanguage` | ❌ |
| `urls` | ❌ (recomendado) |

### Notificación (`administrativeNotice`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `noticeType` | ✅ |
| `issuedAt` | ✅ |
| `state` | ✅ |
| `actSubjectByLanguage` | ✅ |
| `readedAt` | ❌ |
| `urls` | ❌ (recomendado) |

### Registro Oficial (`administrativeOfficialRegisterRecord`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `registeredAt` | ✅ |
| `subjectByLanguage` | ✅ |
| `state` | ✅ |
| `urls` | ❌ (recomendado) |

### Pago único (`oneOffPayment`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `paymentType` | ✅ |
| `paymentSubjectByLanguage` | ✅ |
| `paymentDates` | ✅ |
| `amountInEuro` | ✅ |
| `data.forStatus` | ✅ |
| `amountInEuroIfSurcharged` | ❌ |
| `data.medium` | ❌ |
| `data.device` | ❌ |
| `data.at` | ❌ |
| `data.paymentProcessorId` | ❌ |
| `data.paymentProcessorTransactionId` | ❌ |
| `urls` | ❌ (recomendado) |

### Domiciliación (`directDebitPayment`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `paymentType` | ✅ |
| `paymentSubjectByLanguage` | ✅ |
| `directDebitData` | ✅ |
| `nextChargeAt` | ❌ |
| `nextChargeAmountInEuro` | ❌ |
| `history` | ❌ |
| `urls` | ❌ |

### Cita (`scheduleItem`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `year` | ✅ |
| `monthOfYear` | ✅ |
| `dayOfMonth` | ✅ |
| `hourOfDay` | ✅ |
| `minuteOfHour` | ✅ |
| `durationMinutes` | ✅ |
| `subject` | ✅ |
| `priority` | ❌ |
| `details` | ❌ |
| `location` | ❌ (recomendado) |
| `urls` | ❌ |

### Datos de persona (`personData`)

| Campo | Obligatorio |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `contactData` | ✅ |
| `contactData.partyId` | ✅ |
| `contactData.partyName` | ✅ |
| `contactData.partySurName` | ✅ |
| `contactData.birthDate` | ❌ |
| `contactData.phone` | ❌ |
| `contactData.email` | ❌ |
| `contactData.contactLanguage` | ❌ |
| `contactData.contactMode` | ❌ |
| `addresses` | ❌ |
| `bankDataCollection` | ❌ |

---

## Resumen de formatos

| Tipo de dato | Formato | Ejemplo |
|--------------|---------|---------|
| Fecha/hora (Instant) | ISO 8601 con zona | `2024-03-15T10:30:00Z` |
| Fecha (LocalDate) | `YYYY-MM-DD` | `2024-06-30` |
| Importe | Número con punto decimal | `45.50` |
| Idioma | Enum string | `SPANISH`, `BASQUE`, `ENGLISH` |
| URL | HTTPS válida | `https://sede.miadmin.eus/...` |
| Identificador | String alfanumérico | `EXP-2024-00123` |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
