# :material-tag: DataTypeRef

## Descripción

Objeto para referenciar a un tipo de dato gestionado por DENA (ej: Expediente, Notificación, Pago...).

!!! info "Al menos uno obligatorio"

    Se debe incluir `oid` **o** `id` (o ambos). Si se incluyen los dos, `oid` tiene prioridad.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Identificador interno del tipo de dato |
| `id` | `String` | :material-close:* | Identificador textual del tipo de dato |

---

## Ejemplo

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Valores estándar de `id`

| `id` | Tipo de dato |
|---|---|
| `RECORDS` | Expedientes |
| `NOTICES` | Notificaciones |
| `REGISTRY` | Registros oficiales |
| `PAYMENTS` | Pagos |
| `SCHEDULE` | Citas |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
