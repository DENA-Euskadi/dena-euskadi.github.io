# :material-account: PersonRef

## Descripción

Referencia a una **persona** registrada en DENA. Es una especialización de [DenaObjectRef](./object-ref.md) (concretamente de `DN00DENAObjectWithIDRefBase`), por lo que hereda los campos `oid` e `id` y no añade campos propios.

Clase: `DN00PersonRef` (`@MarshallType(as="personRef")`).

!!! info "Al menos uno obligatorio"
    Se debe incluir `id` **o** `oid` (o ambos). Si se incluyen los dos, `oid` tiene prioridad.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Identificador único de la persona en el módulo de personas de DENA |
| `id` | `ID` | :material-close:* | Identificador administrativo de la persona (DNI / NIF / NIE / Pasaporte) |

!!! note "* Al menos uno"
    Debe incluirse al menos uno de `oid` o `id`.

---

## Ejemplo

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "id": "12345678A"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
