# :material-cube-outline: DenaObjectRef

## Descripción

Tipo base para referenciar cualquier **objeto** de DENA (persona, administración, tipo de dato, etc.). Toda referencia a un objeto en DENA se identifica, como mínimo, por su OID.

La jerarquía de referencias tiene dos niveles base:

| Nivel | Clase | Aporta |
|---|---|---|
| Referencia por OID | `DN00DENAObjectRefBase` | El campo `oid` |
| Referencia por OID + ID | `DN00DENAObjectWithIDRefBase` | Añade el campo `id` |

De `DN00DENAObjectWithIDRefBase` heredan las referencias concretas: [PersonRef](./person-ref.md), [OrgAdminRef](./org-admin-ref.md) y [DataTypeRef](./data-type-ref.md).

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `OID` | :material-check: | Identificador único del objeto en DENA |

!!! info "El `id` llega con la especialización"
    `DN00DENAObjectRefBase` solo define `oid`. El campo `id` (identificador de negocio) lo aporta la clase intermedia `DN00DENAObjectWithIDRefBase`, de la que heredan las referencias concretas.

---

## Ejemplo

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Especializaciones

<div class="grid cards" markdown>

-   :material-account:{ .lg .middle } **PersonRef**

    ---

    Referencia a una persona (`oid` + `id`).

    [:octicons-arrow-right-24: Ver modelo](./person-ref.md)

-   :material-domain:{ .lg .middle } **OrgAdminRef**

    ---

    Referencia a una administración (`oid` + `id` + `dir3Id`).

    [:octicons-arrow-right-24: Ver modelo](./org-admin-ref.md)

-   :material-tag-outline:{ .lg .middle } **DataTypeRef**

    ---

    Referencia a un tipo de dato (`oid` + `id`).

    [:octicons-arrow-right-24: Ver modelo](./data-type-ref.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
