# :material-cube-outline: DenaObjectRef

## Descripción

Tipo base para referenciar cualquier **objeto** de DENA (persona, consentimiento, administración, etc.). Todo objeto en DENA tiene al menos un identificador único, timestamps de ciclo de vida y una URL de acceso.

`DenaObjectRef` es la clase base de la que heredan [DenaOrgRef](./org-admin-ref.md) y [DenaPersonRef](./person-ref.md).

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `objectOid` | `OID` | :material-check: | Identificador único del objeto en DENA |
| `createTS` | `TimeStamp` | :material-close: | Instante en el que se creó el objeto en DENA |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Instante de la última modificación del objeto |
| `deleteTS` | `TimeStamp` | :material-close: | Instante en el que se eliminó el objeto (si aplica) |
| `url` | `URL` | :material-close: | URL con los datos completos del objeto (requiere autorización) |

---

## Ejemplo

```json
{
  "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "createTS": 1670374400,
  "lastUpdateTS": 1680500000,
  "deleteTS": null,
  "url": "https://interop.api.dena.eus/objects/6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Especializaciones

<div class="grid cards" markdown>

-   :material-domain:{ .lg .middle } **DenaOrgRef**

    ---

    Extiende DenaObjectRef con datos de una administración.

    [:octicons-arrow-right-24: Ver modelo](./org-admin-ref.md)

-   :material-account:{ .lg .middle } **DenaPersonRef**

    ---

    Extiende DenaObjectRef con datos de una persona.

    [:octicons-arrow-right-24: Ver modelo](./person-ref.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
