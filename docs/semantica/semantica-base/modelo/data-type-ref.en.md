# :material-tag: DataTypeRef

## Description

Object for referencing a data type managed by DENA (e.g. Record, Notification, Payment...).

!!! info "At least one mandatory"

    Either `oid` **or** `id` must be included (or both). If both are included, `oid` takes priority.

---

## JSON attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Internal identifier of the data type (`DN00DataTypeOID`) |
| `id` | `ID` | :material-close:* | Textual identifier of the data type (`DN00DataTypeID`) |

Class: `DN00DataTypeRef` (`@MarshallType(as="dataTypeRef")`), a specialization of `DN00DENAObjectWithIDRefBase`.

---

## Example

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Values of the `DN00DataTypeEnum` enum

The DATA-RETRIEVE data types are defined in the `DN00DataTypeEnum` enum. Each type's `id` value matches the marshallTypeId of the corresponding data object:

| Enum value | `id` (marshallTypeId) | Data object |
|---|---|---|
| `ADMINISTRATIVE_NOTICE` | `administrativeNotice` | Notification |
| `ADMINISTRATIVE_RECORD` | `administrativeServiceProcedureRecord` | Record |
| `ADMINISTRATIVE_REGISTER` | `administrativeOfficialRegisterRecord` | Official register |
| `PAYMENT_ONE_OFF_PAYMENT` | `oneOffPayment` | One-off payment |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | `directDebitPayment` | Direct debit |
| `SCHEDULE` | `scheduleItem` | Appointment |
| `PERSON_DATA` | `personData` | Person data |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
