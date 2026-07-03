# Balioztapenak eta Formatu-arauak

## Deskribapena

Dokumentu honek DATA-RETRIEVE endpoint-ean administrazioak itzulitako datuek bete behar dituzten balioztapen-arauak, espero diren formatuak eta murrizketak deskribatzen ditu.

---

## Identifikatzaileak

### OID (Identifikatzaile teknikoa)

| Araua | Xehetasuna |
|-------|---------| 
| Mota | `String` |
| Nahitaezkoa | ✅ Bai |
| Formatua | Alfanumeriko librea, zuriunerik gabe |
| Gehienezko luzera | 255 karaktere |
| Bakartasuna | Bakarra objektu-motako administrazioaren barruan |
| Adibidea | `EXP-OID-001`, `NOT-OID-456`, `PAY-OID-789` |

### ID (Negozio-identifikatzailea)

| Araua | Xehetasuna |
|-------|---------| 
| Mota | `String` |
| Nahitaezkoa | ✅ Bai |
| Formatua | Alfanumerikoa, marratxoak eta puntuak izan ditzake |
| Gehienezko luzera | 255 karaktere |
| Bakartasuna | Bakarra objektu-motako administrazioaren barruan |
| Adibidea | `EXP-2024-00123`, `NOT-2024-00456` |

---

## Datak eta orduak

### ISO 8601 formatua (`Instant` eremuak)

`createdAt`, `lastUpdatedAt`, `issuedAt`, `readedAt`, `registeredAt` bezalako eremuak.

| Araua | Xehetasuna |
|-------|---------| 
| Formatua | ISO 8601 ordu-eremuarekin |
| Ordu-eremua | UTC (`Z`) gomendatzen da, baina offset-a (`+02:00`) onartzen da |
| Adibide baliodunak | `2024-03-15T10:30:00Z`, `2024-03-15T12:30:00+02:00` |
| Adibide baliogabeak | `2024-03-15`, `15/03/2024 10:30` |

### Data-formatua (`LocalDate` eremuak)

`dueDate`, `surchargedAt`, `paidAt`, `setAt`, `expiresAt`, `nextChargeAt` bezalako eremuak.

| Araua | Xehetasuna |
|-------|---------| 
| Formatua | `YYYY-MM-DD` |
| Adibide baliodunak | `2024-06-30`, `2024-07-15` |
| Adibide baliogabeak | `30/06/2024`, `2024-6-30` |

---

## Hizkuntza anitzeko testuak (`LanguageTexts`)

| Araua | Xehetasuna |
|-------|---------| 
| Mota | JSON objektua hizkuntza-gakoekin |
| Nahitaezko hizkuntzak | `SPANISH` eta `BASQUE` (gutxienez) |
| Aukerako hizkuntzak | `ENGLISH` |
| Gehienezko luzera testuko | Muga zorrotzik gabe, baina < 500 karaktere gomendatzen da |
| Balio hutsak | Kate hutsak (`""`) edo zuriuneak soilik ez dira onartzen |

```json
{
  "SPANISH": "Texto en castellano",
  "BASQUE": "Testua euskaraz"
}
```

### Hizkuntza-gako baliodunak

| Gakoa | Hizkuntza |
|-------|--------|
| `SPANISH` | Gaztelania |
| `BASQUE` | Euskara |
| `ENGLISH` | Ingelesa |

---

## URLak

| Araua | Xehetasuna |
|-------|---------| 
| Protokoloa | `https://` soilik (`http://` onartzen da proba-inguruneetan) |
| Formatua | RFC 3986 araberako URL baliogarria |
| Irisgarritasuna | URLak herritarrarentzat irisgarria izan behar du |

---

## Diru-kopuruak

| Araua | Xehetasuna |
|-------|---------| 
| Mota | `Number` (puntu dezimala) |
| Moneta | Beti euroak (€) |
| Zehaztasuna | Gehienez 2 dezimal |
| Adibide baliodunak | `45.50`, `120.00`, `0.99` |
| Adibide baliogabeak | `45,50` (koma), `45.505` (3 dezimal) |

---

## Koherentzia-arauak

### Espedientea

| Araua | Xehetasuna |
|-------|---------| 
| `createdAt` ≤ `lastUpdatedAt` | Sorrera-data ezin da azken eguneraketa baino beranduagokoa izan |
| `state.stateCode` = `CLOSED` | Itxita badago, `lastUpdatedAt`-ek itxiera-data islatu behar du |

### Jakinarazpena

| Araua | Xehetasuna |
|-------|---------| 
| `readedAt` = `null` baldin `state` = `PENDING_TO_BE_READED_BY_DESTINATION` | Ezin du irakurketa-datarik izan zain badago |
| `readedAt` ≠ `null` baldin `state` = `ACKNOWLEDGED_BY_DESTINATION` edo `REJECTED_BY_DESTINATION` | Irakurketa-data izan behar du irakurri/baztertu bada |
| `issuedAt` ≤ `readedAt` | Jaulkipen-data ezin da irakurketa-data baino beranduagokoa izan |

### Ordainketa bakarra

| Araua | Xehetasuna |
|-------|---------| 
| `data.forStatus` = `COMPLETED` → `paymentDates.paidAt` ≠ `null` | Osatuta badago, ordainketa-data izan behar du |
| `data.forStatus` = `PENDING` → `paymentDates.paidAt` = `null` | Zain badago, ez du ordainketa-datarik izan behar |
| `amountInEuro` > 0 | Zenbatekoa positiboa izan behar da |
| `amountInEuroIfSurcharged` ≥ `amountInEuro` | Errekargudun zenbatekoa ezin da normala baino txikiagoa izan |
| `paymentDates.dueDate` ≤ `paymentDates.surchargedAt` | Iraungitzea errekarguaren hasiera baino lehenagokoa da |

### Helbideratze bankarioa

| Araua | Xehetasuna |
|-------|---------| 
| `directDebitData.expiresAt` = `null` edo ≥ uneko data | Iraungitzea badu, ez du dagoeneko iraungita egon behar (historikoak salbu) |
| `nextChargeAmountInEuro` > 0 | Hurrengo karguaren zenbatekoa positiboa izan behar da |

### Hitzordua

| Araua | Xehetasuna |
|-------|---------| 
| `monthOfYear` 1 eta 12 artean | Hilabete baliogarria |
| `dayOfMonth` 1 eta 31 artean | Hilabetearentzako egun baliogarria |
| `hourOfDay` 0 eta 23 artean | Ordu baliogarria |
| `minuteOfHour` 0 eta 59 artean | Minutu baliogarria |
| `durationMinutes` ≥ 0 | Iraupena ez-negatiboa (0 = mugarria) |

---

## Eremu nahitaezkoak objektu-motaren arabera

### Espedientea (`administrativeServiceProcedureRecord`)

| Eremua | Nahitaezkoa |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `service` | ✅ |
| `procedure` | ✅ |
| `createdAt` | ✅ |
| `state` | ✅ |
| `lastUpdatedAt` | ❌ |
| `descriptionByLanguage` | ❌ |
| `urls` | ❌ (gomendatua) |

### Jakinarazpena (`administrativeNotice`)

| Eremua | Nahitaezkoa |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `noticeType` | ✅ |
| `issuedAt` | ✅ |
| `state` | ✅ |
| `actSubjectByLanguage` | ✅ |
| `readedAt` | ❌ |
| `urls` | ❌ (gomendatua) |

### Erregistro Ofiziala (`administrativeOfficialRegisterRecord`)

| Eremua | Nahitaezkoa |
|-------|:-----------:|
| `oid` | ✅ |
| `id` | ✅ |
| `procedureRecord` | ✅ |
| `registeredAt` | ✅ |
| `subjectByLanguage` | ✅ |
| `state` | ✅ |
| `urls` | ❌ (gomendatua) |

### Ordainketa bakarra (`oneOffPayment`)

| Eremua | Nahitaezkoa |
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
| `urls` | ❌ (gomendatua) |

### Helbideratze bankarioa (`directDebitPayment`)

| Eremua | Nahitaezkoa |
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

### Hitzordua (`scheduleItem`)

| Eremua | Nahitaezkoa |
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
| `location` | ❌ (gomendatua) |
| `urls` | ❌ |

### Pertsonaren datuak (`personData`)

| Eremua | Nahitaezkoa |
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

## Formatuen laburpena

| Datu-mota | Formatua | Adibidea |
|------------|---------|---------| 
| Data/ordua (Instant) | ISO 8601 eremuarekin | `2024-03-15T10:30:00Z` |
| Data (LocalDate) | `YYYY-MM-DD` | `2024-06-30` |
| Zenbatekoa | Puntu dezimaldun zenbakia | `45.50` |
| Hizkuntza | Enum string | `SPANISH`, `BASQUE`, `ENGLISH` |
| URLa | HTTPS baliogarria | `https://sede.miadmin.eus/...` |
| Identifikatzailea | Kate alfanumerikoa | `EXP-2024-00123` |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
