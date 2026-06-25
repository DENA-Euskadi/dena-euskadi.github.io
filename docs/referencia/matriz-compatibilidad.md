# :material-table-check: Matriz de compatibilidad

¿Qué endpoints son obligatorios u opcionales según tu tipo de integración?

---

## Por caso de uso

| Capacidad | Data-Retrieve | Metadata-Sync | Person-Sync Pull | Person-Sync Push |
|---|:---:|:---:|:---:|:---:|
| **DENA consulta datos de mi administración** | :material-check-bold:{ title="Obligatorio" } | :material-minus:{ title="Opcional" } | :material-minus:{ title="Opcional" } | :material-minus:{ title="Opcional" } |
| **Notifico a DENA cuando hay cambios** | :material-close:{ title="No aplica" } | :material-check-bold:{ title="Obligatorio" } | :material-minus:{ title="Opcional" } | :material-minus:{ title="Opcional" } |
| **Mantengo listado de personas (batch)** | :material-close:{ title="No aplica" } | :material-close:{ title="No aplica" } | :material-check-bold:{ title="Obligatorio" } | :material-close:{ title="No aplica" } |
| **Recibo personas en tiempo real** | :material-close:{ title="No aplica" } | :material-close:{ title="No aplica" } | :material-close:{ title="No aplica" } | :material-check-bold:{ title="Obligatorio" } |
| **Integración completa** | :material-check-bold:{ title="Obligatorio" } | :material-check-bold:{ title="Obligatorio" } | :material-check-bold:{ title="Obligatorio" } | :material-minus:{ title="Opcional" } |

!!! note "Leyenda"

    - :material-check-bold: — Obligatorio para este caso
    - :material-minus: — Opcional (recomendado pero no necesario)
    - :material-close: — No aplica

---

## Por rol (quién implementa qué)

| Endpoint | Quién lo implementa | Quién lo invoca |
|---|---|---|
| `POST /api/retrieveData` | :material-domain: Administración | :material-swap-horizontal: DENA |
| `POST /syncMetadata` (DENA) | :material-swap-horizontal: DENA | :material-domain: Administración |
| `GET /persons/export` (DENA) | :material-swap-horizontal: DENA | :material-domain: Administración |
| `POST /api/person-push` | :material-domain: Administración | :material-swap-horizontal: DENA |

---

## Por tipo de dato

| dataTypeId | Objeto devuelto | Documentación |
|---|---|---|
| `RECORDS` | Expedientes | [expediente.md](../semantica/data-retrieve/data/expediente.md) |
| `NOTICES` | Notificaciones | [notificacion.md](../semantica/data-retrieve/data/notificacion.md) |
| `REGISTRY` | Registros oficiales | [registro-oficial.md](../semantica/data-retrieve/data/registro-oficial.md) |
| `PAYMENTS` | Pagos | [pago.md](../semantica/data-retrieve/data/pago.md) |
| `SCHEDULE` | Citas | [cita.md](../semantica/data-retrieve/data/cita.md) |

!!! tip "No necesitas implementar todos los tipos"

    Implementa solo los tipos de dato que tu administración gestiona.
    Si te piden un tipo que no tienes, devuelve `dataItems: []` con `code: "OK"`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
