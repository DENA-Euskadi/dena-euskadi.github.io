# :material-tag: DataTypeRef

## Description

`dataType` is the `context` field that indicates **which type of data** is being requested or exchanged (Record, Notification, Payment...). It is the piece an administration reads to know which object to return.

!!! tip "All an administration needs"

    To implement the endpoint, **it is enough to read `dataType.id`**: it is a catalog string (e.g. `administrativeServiceProcedureRecord`) that identifies the data type. Based on its value, you return the corresponding object. The `oid` is a DENA-internal identifier and **does not need to be interpreted**.

---

## The three pieces (and why they exist)

The model separates three concepts that are often confused. In practice you will only work with the `id`:

| Piece | Java class | What it is | Example |
|---|---|---|---|
| **`id`** | `DN00DataTypeID` (`@MarshallType(as="dataTypeId")`) | **Textual** identifier of the data type. It is the catalog value and matches the object's `marshallTypeId`. **This is what you interpret.** | `"administrativeServiceProcedureRecord"` |
| **`oid`** | `DN00DataTypeOID` (`@MarshallType(as="dataTypeOid")`) | **Internal** DENA identifier (a GUID). Internal use; an administration does not need it. | `"6AE83A0C-2202-4666-9857-3334C14663A2"` |
| **`dataType`** (container) | `DN00DataTypeRef` (`@MarshallType(as="dataTypeRef")`) | The object that groups `oid` + `id` and travels inside the `context`. Specializes `DN00DENAObjectWithIDRefBase`. | `{ "id": "...", "oid": "..." }` |

!!! info "`oid` or `id`?"

    Either `oid` **or** `id` must be included (or both). In DATA-RETRIEVE, DENA always sends the `id`, which is the one you should use. If both come, `oid` takes priority internally, but the `id` is always enough to decide which object to return.

---

## JSON attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `id` | `String` | :material-check:* | Textual identifier of the data type (`DN00DataTypeID`). One of the catalog values (see table below) |
| `oid` | `String` | :material-close:* | DENA-internal identifier (`DN00DataTypeOID`, a GUID). Internal use |

<small>*At least one of the two. In practice the `id` always arrives.</small>

---

## Example

```json
{
    "id": "administrativeServiceProcedureRecord",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

> The `id` is the one you use; the `oid` (an internal GUID) may or may not come and you do not need to interpret it.

---

## Data type catalog (`id`)

The valid `id` values are defined in the `DN00DataTypeEnum` enum. Each value matches the `marshallTypeId` of the corresponding data object, so the `id` tells you directly which object to return:

| `id` (value of `dataType.id`) | Data object to return | `DN00DataTypeEnum` constant |
|---|---|---|
| `administrativeServiceProcedureRecord` | Record | `ADMINISTRATIVE_RECORD` |
| `administrativeNotice` | Notification | `ADMINISTRATIVE_NOTICE` |
| `administrativeOfficialRegisterRecord` | Official register | `ADMINISTRATIVE_REGISTER` |
| `oneOffPayment` | One-off payment | `PAYMENT_ONE_OFF_PAYMENT` |
| `directDebitPayment` | Direct debit | `PAYMENT_DIRECT_DEBIT_PAYMENT` |
| `scheduleItem` | Appointment | `SCHEDULE` |
| `personData` | Person data | `PERSON_DATA` |

> The `DN00DataTypeEnum` enum lives in `dena-common-data-api`; the identifiers (`DN00DataTypeID`/`DN00DataTypeOID`) and the `DN00DataTypeRef` container live in `dena-common-api`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
