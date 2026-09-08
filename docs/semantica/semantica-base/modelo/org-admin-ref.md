# :material-domain: OrgAdminRef

## Descripción

Referencia a una **administración**. Es una especialización de [DenaObjectRef](./object-ref.md) (concretamente de `DN00DENAObjectWithIDRefBase`): hereda `oid` e `id`, y añade el campo propio `dir3Id`.

Clase: `DN00OrgAdminRef`.

!!! info "Envío simplificado"
    Cuando una administración envía esta referencia, basta con uno de los campos de identificación (`oid`, `id` o `dir3Id`). DENA obtiene el resto de su directorio interno de entidades.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Identificador único de la administración en el módulo de organización de DENA |
| `id` | `ID` | :material-close:* | Identificador de la administración (ej: NIF) |
| `dir3Id` | `ID` | :material-close:* | Código DIR3 de la administración |

!!! note "* Al menos uno"
    Debe incluirse al menos uno de `oid`, `id` o `dir3Id`.

---

## Ejemplo

```json
{
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "id": "S4833001C",
  "dir3Id": "EA0000001"
}
```

---

## Estructura organizativa

Cuando es necesario enviar una **estructura organizativa** (organigrama jerárquico), se puede enviar un `Array` de referencias donde el orden marca el nivel jerárquico:

- Elemento `[0]`: primer nivel de la organización
- Elemento `[1]`: segundo nivel
- ...

```json
[
  { "id": "S4833001C", "dir3Id": "EA0000001" },
  { "id": "S4811001J", "dir3Id": "EA0041020" }
]
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
