# Validations and Format Rules

## Description

This document describes the validation rules, expected formats and restrictions that must be met by the data returned by the administration in the DATA-RETRIEVE endpoint.

---

## Identifiers

### OID (Technical identifier)

| Rule | Detail |
|-------|---------| 
| Type | `String` |
| Mandatory | ✅ Yes |
| Format | Free alphanumeric, no spaces |
| Maximum length | 255 characters |
| Uniqueness | Unique per object type within the administration |
| Example | `EXP-OID-001`, `NOT-OID-456`, `PAY-OID-789` |

### ID (Business identifier)

| Rule | Detail |
|-------|---------| 
| Type | `String` |
| Mandatory | ✅ Yes |
| Format | Alphanumeric, may include hyphens and dots |
| Maximum length | 255 characters |
| Uniqueness | Unique per object type within the administration |
| Example | `EXP-2024-00123`, `NOT-2024-00456` |

---

## Dates and times

### ISO 8601 format (`Instant` fields)

Fields such as `createdAt`, `lastUpdatedAt`, `issuedAt`, `readedAt`, `registeredAt`.

| Rule | Detail |
|-------|---------| 
| Format | ISO 8601 with time zone |
| Time zone | UTC (`Z`) is recommended, but offset (`+02:00`) is accepted |
| Valid examples | `2024-03-15T10:30:00Z`, `2024-03-15T12:30:00+02:00` |
| Invalid examples | `2024-03-15`, `15/03/2024 10:30` |

### Date format (`LocalDate` fields)

Fields such as `dueDate`, `surchargedAt`, `paidAt`, `setAt`, `expiresAt`, `nextChargeAt`.

| Rule | Detail |
|-------|---------| 
| Format | `YYYY-MM-DD` |
| Valid examples | `2024-06-30`, `2024-07-15` |
| Invalid examples | `30/06/2024`, `2024-6-30` |

---

## Multilingual texts (`LanguageTexts`)

| Rule | Detail |
|-------|---------| 
| Type | JSON object with language keys |
| Mandatory languages | `SPANISH` and `BASQUE` (minimum) |
| Optional languages | `ENGLISH` |
| Maximum length per text | No strict limit, but < 500 characters is recommended |
| Empty values | Empty strings (`""`) or whitespace-only are not allowed |

```json
{
  "SPANISH": "Texto en castellano",
  "BASQUE": "Testua euskaraz"
}
```

### Valid language keys

| Key | Language |
|-------|--------|
| `SPANISH` | Spanish |
| `BASQUE` | Basque |
| `ENGLISH` | English |

---

## URLs

| Rule | Detail |
|-------|---------| 
| Protocol | Only `https://` (`http://` is accepted in test environments) |
| Format | Valid URL according to RFC 3986 |
| Accessibility | The URL must be accessible to the citizen |

---

## Monetary amounts

| Rule | Detail |
|-------|---------| 
| Type | `Number` (decimal point) |
| Currency | Always euros (€) |
| Precision | Maximum 2 decimal places |
| Valid examples | `45.50`, `120.00`, `0.99` |
| Invalid examples | `45,50` (comma), `45.505` (3 decimals) |

---

## Coherence rules

### Record

| Rule | Detail |
|-------|---------| 
| `createdAt` ≤ `lastUpdatedAt` | The creation date cannot be later than the last update |
| `state.stateCode` = `CLOSED` | If closed, `lastUpdatedAt` must reflect the closing date |

### Notification

| Rule | Detail |
|-------|---------| 
| `readedAt` = `null` if `state` = `PENDING_TO_BE_READED_BY_DESTINATION` | Cannot have a read date if pending |
| `readedAt` ≠ `null` if `state` = `ACKNOWLEDGED_BY_DESTINATION` or `REJECTED_BY_DESTINATION` | Must have a read date if read/rejected |
| `issuedAt` ≤ `readedAt` | The issue date cannot be later than the read date |

### One-off payment

| Rule | Detail |
|-------|---------| 
| `data.forStatus` = `COMPLETED` → `paymentDates.paidAt` ≠ `null` | If completed, must have a payment date |
| `data.forStatus` = `PENDING` → `paymentDates.paidAt` = `null` | If pending, must not have a payment date |
| `amountInEuro` > 0 | The amount must be positive |
| `amountInEuroIfSurcharged` ≥ `amountInEuro` | The surcharged amount cannot be less than the normal one |
| `paymentDates.dueDate` ≤ `paymentDates.surchargedAt` | The due date is before the surcharge start |

### Direct debit

| Rule | Detail |
|-------|---------| 
| `directDebitData.expiresAt` = `null` or ≥ current date | If it has an expiration, it must not already be expired (except for historical records) |
| `nextChargeAmountInEuro` > 0 | The next charge amount must be positive |

### Appointment

| Rule | Detail |
|-------|---------| 
| `monthOfYear` between 1 and 12 | Valid month |
| `dayOfMonth` between 1 and 31 | Valid day for the month |
| `hourOfDay` between 0 and 23 | Valid hour |
| `minuteOfHour` between 0 and 59 | Valid minute |
| `durationMinutes` ≥ 0 | Non-negative duration (0 = milestone) |

---

## Mandatory fields by object type

### Record (`administrativeServiceProcedureRecord`)

| Field | Mandatory |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `service` | ✅ |
| `procedure` | ✅ |
| `createdAt` | ✅ |
| `state` | ✅ |
| `lastUpdatedAt` | ❌ |
| `descriptionByLanguage` | ❌ |
| `urls` | ❌ (recommended) |

### Notification (`administrativeNotice`)

| Field | Mandatory |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `noticeType` | ✅ |
| `issuedAt` | ✅ |
| `state` | ✅ |
| `actSubjectByLanguage` | ✅ |
| `readedAt` | ❌ |
| `urls` | ❌ (recommended) |

### Official Registry (`administrativeOfficialRegisterRecord`)

| Field | Mandatory |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `registeredAt` | ✅ |
| `subjectByLanguage` | ✅ |
| `state` | ✅ |
| `urls` | ❌ (recommended) |

### One-off payment (`oneOffPayment`)

| Field | Mandatory |
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
| `urls` | ❌ (recommended) |

### Direct debit (`directDebitPayment`)

| Field | Mandatory |
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

### Appointment (`scheduleItem`)

| Field | Mandatory |
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
| `location` | ❌ (recommended) |
| `urls` | ❌ |

### Person data (`personData`)

| Field | Mandatory |
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

## Format summary

| Data type | Format | Example |
|------------|---------|---------| 
| Date/time (Instant) | ISO 8601 with zone | `2024-03-15T10:30:00Z` |
| Date (LocalDate) | `YYYY-MM-DD` | `2024-06-30` |
| Amount | Number with decimal point | `45.50` |
| Language | Enum string | `SPANISH`, `BASQUE`, `ENGLISH` |
| URL | Valid HTTPS | `https://sede.miadmin.eus/...` |
| Identifier | Alphanumeric string | `EXP-2024-00123` |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
