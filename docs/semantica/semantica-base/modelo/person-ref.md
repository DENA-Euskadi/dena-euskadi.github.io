# :material-account: PersonRef

## Descripción

Objeto para referenciar a una persona registrada en DENA por su `oid` o `id` (NIF, NIE, etc).

!!! info "Al menos uno obligatorio"

    Se debe incluir `oid` **o** `id` (o ambos).

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `String` | :material-close:* | Identificador interno de la persona |
| `id` | `String` | :material-close:* | Identificador externo (NIF, NIE, etc) |

---

## Ejemplo

```json
{
    "id": "12345678A",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
