# :material-tag: DataTypeRef

## Deskribapena

`dataType` `context`-eko eremua da eta **zein datu mota** eskatzen edo trukatzen den adierazten du (Espedientea, Jakinarazpena, Ordainketa...). Administrazio batek zein objektu itzuli behar duen jakiteko irakurtzen duen zatia da.

!!! tip "Administrazio batek behar duen guztia"

    Endpoint-a inplementatzeko, **nahikoa da `dataType.id` irakurtzea**: datu mota identifikatzen duen katalogoko string bat da (adib. `administrativeServiceProcedureRecord`). Bere balioaren arabera, dagokion objektua itzultzen duzu. `oid` DENAren barne-identifikatzailea da eta **ez da interpretatu behar**.

---

## Hiru zatiak (eta zergatik dauden)

Ereduak sarritan nahasten diren hiru kontzeptu bereizten ditu. Praktikan `id`-rekin bakarrik lan egingo duzu:

| Zatia | Java klasea | Zer den | Adibidea |
|---|---|---|---|
| **`id`** | `DN00DataTypeID` (`@MarshallType(as="dataTypeId")`) | Datu motaren **testu**-identifikatzailea. Katalogoko balioa da eta objektuaren `marshallTypeId`-arekin bat egiten du. **Hau da interpretatzen duzuna.** | `"administrativeServiceProcedureRecord"` |
| **`oid`** | `DN00DataTypeOID` (`@MarshallType(as="dataTypeOid")`) | DENAren **barne**-identifikatzailea (GUID bat). Barne-erabilerakoa; administrazio batek ez du behar. | `"6AE83A0C-2202-4666-9857-3334C14663A2"` |
| **`dataType`** (edukiontzia) | `DN00DataTypeRef` (`@MarshallType(as="dataTypeRef")`) | `oid` + `id` biltzen dituen objektua, `context`-aren barruan bidaltzen dena. `DN00DENAObjectWithIDRefBase` espezializatzen du. | `{ "id": "...", "oid": "..." }` |

!!! info "`oid` edo `id`?"

    `oid` **edo** `id` sartu behar da (edo biak). DATA-RETRIEVE-n, DENAk beti bidaltzen du `id`, erabili behar duzuna. Biak etortzen badira, `oid`-k lehentasuna du barne-mailan, baina `id` beti da nahikoa zein objektu itzuli erabakitzeko.

---

## JSON atributuak

| Eremua | Mota | Derrigorrez | Deskribapena |
|---|---|:---:|---|
| `id` | `String` | :material-check:* | Datu motaren testu-identifikatzailea (`DN00DataTypeID`). Katalogoko balioetako bat (ikusi beheko taula) |
| `oid` | `String` | :material-close:* | DENAren barne-identifikatzailea (`DN00DataTypeOID`, GUID bat). Barne-erabilerakoa |

<small>*Gutxienez bietako bat. Praktikan `id` beti iristen da.</small>

---

## Adibidea

```json
{
    "id": "administrativeServiceProcedureRecord",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

> `id` da erabiltzen duzuna; `oid` (barne GUID bat) etorri daiteke ala ez eta ez duzu interpretatu behar.

---

## Datu moten katalogoa (`id`)

Baliozko `id` balioak `DN00DataTypeEnum` enum-ean definitzen dira. Balio bakoitzak dagokion datu-objektuaren `marshallTypeId`-arekin bat egiten du, beraz `id`-k zuzenean esaten dizu zein objektu itzuli:

| `id` (`dataType.id`-ren balioa) | Itzuli beharreko datu-objektua | `DN00DataTypeEnum` konstantea |
|---|---|---|
| `administrativeServiceProcedureRecord` | Espedientea | `ADMINISTRATIVE_RECORD` |
| `administrativeNotice` | Jakinarazpena | `ADMINISTRATIVE_NOTICE` |
| `administrativeOfficialRegisterRecord` | Erregistro ofiziala | `ADMINISTRATIVE_REGISTER` |
| `oneOffPayment` | Ordainketa bakarra | `PAYMENT_ONE_OFF_PAYMENT` |
| `directDebitPayment` | Helbideratzea | `PAYMENT_DIRECT_DEBIT_PAYMENT` |
| `scheduleItem` | Hitzordua | `SCHEDULE` |
| `personData` | Pertsonaren datuak | `PERSON_DATA` |

> `DN00DataTypeEnum` enum-a `dena-common-data-api`-n dago; identifikatzaileak (`DN00DataTypeID`/`DN00DataTypeOID`) eta `DN00DataTypeRef` edukiontzia `dena-common-api`-n daude.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
