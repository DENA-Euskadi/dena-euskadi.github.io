!!! danger "⚠️ TIPO OBSOLETO — Usa Libro Registro en su lugar"
    `DN00AdministrativeOfficialRegisterRecord` y el tipo `ADMINISTRATIVE_REGISTER` están **obsoletos**: **no existen** en el enum `DN00DataTypeEnum` de v0.5.20 y **no tienen implementación Java** en el código fuente. No uses este tipo en implementaciones nuevas; cualquier objeto enviado con `"type": "administrativeOfficialRegisterRecord"` será rechazado.

    **El tipo que documenta entradas/salidas de registro oficial es [`libro-registro.md`](./libro-registro.md) (`AdministrativeLedger` / `administrativeLedger`).**

# :material-book-open-page-variant: Registro Oficial (Official Register Record) — OBSOLETO

> - **Estado:** ❌ Obsoleto / sin implementación en v{{ dena.version }}
> - **Sustituto:** [Libro Registro](./libro-registro.md) (`administrativeLedger`)

## ¿Por qué está obsoleto?

Este tipo documentaba un asiento registral de entrada/salida vinculado a un expediente. En
v0.5.20 su funcionalidad la cubre el **Libro Registro** (`administrativeLedger`), que modela las
entradas y salidas del registro mediante el campo `entryDirection` (`ENTRY` / `EXIT`).

El tipo `administrativeOfficialRegisterRecord`:

- **No está** en el catálogo `DN00DataTypeEnum` (ver [DataTypeRef](../../semantica-base/modelo/data-type-ref.md)).
- **No tiene** clases Java en el código fuente (`DN00AdministrativeOfficialRegisterRecord` y sus
  estados no existen como fuente).

## Qué usar en su lugar

| Antes (obsoleto) | Ahora (v0.5.20) |
|---|---|
| `administrativeOfficialRegisterRecord` | [`administrativeLedger`](./libro-registro.md) |
| Estados `PRESENTED` / `RECEIVED_FROM_OTHER_ORG_UNIT` / `TRANSFERRED_FROM_OTHER_ORG_UNIT` | `entryDirection`: `ENTRY` / `EXIT` |

[:octicons-arrow-right-24: Ir a Libro Registro (`administrativeLedger`)](./libro-registro.md)

---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
