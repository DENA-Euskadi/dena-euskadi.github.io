# :material-domain: OrgAdminRef (DenaOrgRef)

## Descripción

Especialización de [DenaObjectRef](./object-ref.md) con datos mínimos sobre una **administración**. Contiene la información necesaria para identificar de forma unívoca a una administración en DENA.

!!! info "Envío simplificado"
    Cuando una administración envía esta información, **basta con enviar uno de los campos de identificación** (`orgId`, `officialId` o `objectOid`). DENA es capaz de obtener el resto del directorio interno de entidades.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `orgId` | `ID` | :material-close:* | Identificador de la administración (ej: NIF) |
| `officialId` | `ID` | :material-close:* | Código DIR3 de la administración |
| `objectOid` | `OID` | :material-close:* | Identificador único del objeto en el módulo de organización de DENA |
| `createTS` | `TimeStamp` | :material-close: | Instante en el que se creó el objeto en DENA |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Instante de la última modificación |
| `deleteTS` | `TimeStamp` | :material-close: | Instante en el que se eliminó (si aplica) |
| `url` | `URL` | :material-close: | URL con los datos completos de la administración |

!!! info "Al menos uno obligatorio"
    Se debe incluir al menos uno de: `orgId`, `officialId` u `objectOid`.

---

## Ejemplo

```json
{
  "orgId": "S4833001C",
  "officialId": "EA0000001",
  "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "createTS": 1670374400,
  "lastUpdateTS": 1680500000,
  "url": "https://interop.api.dena.eus/orgs/6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Estructura organizativa

Cuando es necesario enviar una **estructura organizativa** (organigrama jerárquico), se puede enviar un `Array` de `DenaOrgRef` donde:

- Elemento `[0]`: primer nivel de la organización
- Elemento `[1]`: segundo nivel
- ...

```json
[
  { "orgId": "S4833001C", "officialId": "EA0000001" },
  { "orgId": "S4811001J", "officialId": "EA0041020" }
]
```

---

## Uso simplificado

Para la mayoría de los mensajes, basta con el formato reducido:

```json
{
  "id": "admin-A414",
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

donde `id` mapea a `orgId` y `oid` mapea a `objectOid`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
