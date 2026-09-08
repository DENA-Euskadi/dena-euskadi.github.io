# :material-account: PersonRef (DenaPersonRef)

## Descripción

Especialización de [DenaObjectRef](./object-ref.md) con datos mínimos sobre una **persona** registrada en DENA.

!!! info "Al menos uno obligatorio"
    Se debe incluir `personId` **o** `objectOid` (o ambos). Si se incluyen los dos, `objectOid` tiene prioridad.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `personId` | `ID` | :material-close:* | Identificador administrativo de la persona (DNI / NIF / NIE / Pasaporte) |
| `objectOid` | `OID` | :material-close:* | Identificador único del objeto en el módulo de personas de DENA |
| `createTS` | `TimeStamp` | :material-close: | Instante en el que se creó el objeto en DENA |
| `lastUpdateTS` | `TimeStamp` | :material-close: | Instante de la última modificación |
| `deleteTS` | `TimeStamp` | :material-close: | Instante en el que se eliminó (si aplica) |
| `url` | `URL` | :material-close: | URL con los datos completos de la persona (requiere autorización) |

---

## Ejemplo completo

```json
{
  "personId": "12345678A",
  "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2",
  "createTS": 1670374400,
  "lastUpdateTS": 1680500000,
  "deleteTS": null,
  "url": "https://interop.api.dena.eus/persons/6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Uso simplificado

Para la mayoría de los mensajes, basta con el formato reducido:

```json
{
  "id": "12345678A",
  "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

donde `id` mapea a `personId` y `oid` mapea a `objectOid`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
