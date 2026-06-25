# :material-domain: OrgAdminRef

## Descripción

Objeto para referenciar a una administración por su `oid` o `id`.

!!! info "Al menos uno obligatorio"

    Se debe incluir `oid` **o** `id` (o ambos).

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Identificador interno de la administración |
| `id` | `String` | :material-close:* | Identificador textual de la administración |

---

## Ejemplo

```json
{
    "id": "admin-A414",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
