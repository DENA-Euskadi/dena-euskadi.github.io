# :material-table-check: Compatibility matrix

Which endpoints are mandatory or optional depending on your integration type?

---

## By use case

| Capability | Data-Retrieve | Metadata-Sync | Person-Sync Pull | Person-Sync Push |
|---|:---:|:---:|:---:|:---:|
| **DENA queries data from my administration** | :material-check-bold:{ title="Mandatory" } | :material-minus:{ title="Optional" } | :material-minus:{ title="Optional" } | :material-minus:{ title="Optional" } |
| **I notify DENA when there are changes** | :material-close:{ title="Not applicable" } | :material-check-bold:{ title="Mandatory" } | :material-minus:{ title="Optional" } | :material-minus:{ title="Optional" } |
| **I maintain a list of persons (batch)** | :material-close:{ title="Not applicable" } | :material-close:{ title="Not applicable" } | :material-check-bold:{ title="Mandatory" } | :material-close:{ title="Not applicable" } |
| **I receive persons in real time** | :material-close:{ title="Not applicable" } | :material-close:{ title="Not applicable" } | :material-close:{ title="Not applicable" } | :material-check-bold:{ title="Mandatory" } |
| **Full integration** | :material-check-bold:{ title="Mandatory" } | :material-check-bold:{ title="Mandatory" } | :material-check-bold:{ title="Mandatory" } | :material-minus:{ title="Optional" } |

!!! note "Legend"

    - :material-check-bold: — Mandatory for this case
    - :material-minus: — Optional (recommended but not required)
    - :material-close: — Not applicable

---

## By role (who implements what)

| Endpoint | Who implements it | Who invokes it |
|---|---|---|
| `POST /api/retrieveData` | :material-domain: Administration | :material-swap-horizontal: DENA |
| `POST /syncMetadata` (DENA) | :material-swap-horizontal: DENA | :material-domain: Administration |
| `GET /persons/export` (DENA) | :material-swap-horizontal: DENA | :material-domain: Administration |
| `POST /api/person-push` | :material-domain: Administration | :material-swap-horizontal: DENA |

---

## By data type

| dataTypeId | Returned object | Documentation |
|---|---|---|
| `RECORDS` | Records | [expediente.md](../semantica/data-retrieve/data/expediente.md) |
| `NOTICES` | Notifications | [notificacion.md](../semantica/data-retrieve/data/notificacion.md) |
| `REGISTRY` | Official registries | [registro-oficial.md](../semantica/data-retrieve/data/registro-oficial.md) |
| `PAYMENTS` | Payments | [pago.md](../semantica/data-retrieve/data/pago.md) |
| `SCHEDULE` | Appointments | [cita.md](../semantica/data-retrieve/data/cita.md) |

!!! tip "You don't need to implement all types"

    Implement only the data types that your administration manages.
    If a type you don't have is requested, return `dataItems: []` with `code: "OK"`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
