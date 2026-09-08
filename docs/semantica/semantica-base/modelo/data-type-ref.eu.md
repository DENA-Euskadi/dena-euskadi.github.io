# :material-tag: DataTypeRef

## Deskribapena

DENAk kudeatutako datu mota bati erreferentzia egiteko objektua (adib. Espedientea, Jakinarazpena, Ordainketa...).

!!! info "Gutxienez bat derrigorrez"

    `oid` **edo** `id` sartu behar da (edo biak). Biak sartzen badira, `oid`-k lehentasuna du.

---

## JSON atributuak

| Eremua | Mota | Derrigorrez | Deskribapena |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Datu motaren barne-identifikatzailea (`DN00DataTypeOID`) |
| `id` | `ID` | :material-close:* | Datu motaren testu-identifikatzailea (`DN00DataTypeID`) |

Klasea: `DN00DataTypeRef` (`@MarshallType(as="dataTypeRef")`), `DN00DENAObjectWithIDRefBase`-ren espezializazioa.

---

## Adibidea

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## `DN00DataTypeEnum` enum-aren balioak

DATA-RETRIEVE datu motak `DN00DataTypeEnum` enum-ean definitzen dira. Mota bakoitzaren `id` balioak dagokion datu-objektuaren marshallTypeId-arekin bat egiten du:

| Enum balioa | `id` (marshallTypeId) | Datu-objektua |
|---|---|---|
| `ADMINISTRATIVE_NOTICE` | `administrativeNotice` | Jakinarazpena |
| `ADMINISTRATIVE_RECORD` | `administrativeServiceProcedureRecord` | Espedientea |
| `ADMINISTRATIVE_REGISTER` | `administrativeOfficialRegisterRecord` | Erregistro ofiziala |
| `PAYMENT_ONE_OFF_PAYMENT` | `oneOffPayment` | Ordainketa bakarra |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | `directDebitPayment` | Helbideratzea |
| `SCHEDULE` | `scheduleItem` | Hitzordua |
| `PERSON_DATA` | `personData` | Pertsonaren datuak |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
