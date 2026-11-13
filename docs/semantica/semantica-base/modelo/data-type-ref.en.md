# :material-tag: DataTypeRef

## Description

The `dataType` field in the `context` indicates **what type of data** is being requested or exchanged. It is the piece your administration reads to know which object it has to return.

!!! tip "The only thing an administration needs"

    **Just read `dataType.id`**: it is a text string (e.g. `SCHEDULE`) that identifies the data type. Based on its value, you return the corresponding object. The `oid` is an internal DENA identifier and **does not need to be interpreted**.

---

## JSON Attributes

| Field | Required | Description |
|---|:---:|---|
| `id` | ✅* | Textual identifier of the data type. This is what you use to know which object to return |
| `oid` | ❌* | Internal DENA identifier (a GUID). For internal use; no need to interpret it |

<small>*At least one of the two. In practice, `id` always arrives.</small>

---

## Example

```json
{
    "id": "SCHEDULE",
    "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4006"
}
```

---

## Data type catalogue (`dataType.id`)

Valid values that can arrive in `dataType.id` and the data object to return:

| `dataType.id` | Data object to return | Documentation |
|---|---|---|
| `ADMINISTRATIVE_RECORD` | Administrative record | [expediente.md](../../../data-retrieve/data/expediente.md) |
| `ADMINISTRATIVE_NOTICE` | Notification | [notificacion.md](../../../data-retrieve/data/notificacion.md) |
| `PAYMENT_ONE_OFF_PAYMENT` | One-off payment | [pago.md](../../../data-retrieve/data/pago.md) |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | Direct debit payment | [pago.md](../../../data-retrieve/data/pago.md) |
| `SCHEDULE` | Appointment | [cita.md](../../../data-retrieve/data/cita.md) |
| `PERSON_DATA` | Person data | [persona.md](../../../data-retrieve/data/persona.md) |
| `PERSON_CREDENTIAL` | Person credential | [credencial-persona.md](../../../data-retrieve/data/credencial-persona.md) |
| `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | Basque language proficiency credential | [credencial-persona.md](../../../data-retrieve/data/credencial-persona.md) |
| `ECONOMIC_BENEFIT` | Economic benefit | [beneficio-economico.md](../../../data-retrieve/data/beneficio-economico.md) |
| `ADMINISTRATIVE_LEDGER` | Administrative ledger | [libro-registro.md](../../../data-retrieve/data/libro-registro.md) |
| `ADMINISTRATIVE_PERSON_REGISTER` | Person register | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | Person register — PEV housing | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | Person register — last will | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | Person register — PEV revision | [registro-persona.md](../../../data-retrieve/data/registro-persona.md) |
| `PERSON_ENROLLMENT` | Enrollment | [matricula.md](../../../data-retrieve/data/matricula.md) |
| `PERSON_ACADEMIC_ENROLLMENT` | Academic enrollment | [matricula.md](../../../data-retrieve/data/matricula.md) |
| `PERSON_MEDICAL_DATA` | Medical data | [datos-medicos.md](../../../data-retrieve/data/datos-medicos.md) |

> ⚠️ The `ADMINISTRATIVE_OFFICIAL_REGISTER_RECORD` type is **deprecated** and has been removed from the catalogue. Its replacement is `ADMINISTRATIVE_LEDGER`.

<!-- DENA-DOC-FOOTER -->
