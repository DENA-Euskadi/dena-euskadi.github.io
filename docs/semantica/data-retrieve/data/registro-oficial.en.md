!!! danger "⚠️ DEPRECATED TYPE — Use Register Ledger instead"
    `DN00AdministrativeOfficialRegisterRecord` and type `ADMINISTRATIVE_REGISTER` are **deprecated**: they **do not exist** in the v0.5.20 `DN00DataTypeEnum` and have **no Java implementation** in the source code. Do not use this type in new implementations; any object sent with `"type": "administrativeOfficialRegisterRecord"` will be rejected.

    **The type that documents register entries/exits is [`libro-registro.md`](./libro-registro.md) (`AdministrativeLedger` / `administrativeLedger`).**

# :material-book-open-page-variant: Official Registration (Official Register Record) — DEPRECATED

> - **Status:** ❌ Deprecated / not implemented in v{{ dena.version }}
> - **Replacement:** [Register Ledger](./libro-registro.md) (`administrativeLedger`)

## Why is it deprecated?

This type documented an inbound/outbound register entry linked to a record. In v0.5.20 its
functionality is covered by the **Register Ledger** (`administrativeLedger`), which models register
entries and exits through the `entryDirection` field (`ENTRY` / `EXIT`).

The `administrativeOfficialRegisterRecord` type:

- Is **not** part of the `DN00DataTypeEnum` catalog (see [DataTypeRef](../../semantica-base/modelo/data-type-ref.md)).
- Has **no** Java classes in the source code (`DN00AdministrativeOfficialRegisterRecord` and its
  states do not exist as source).

## What to use instead

| Before (deprecated) | Now (v0.5.20) |
|---|---|
| `administrativeOfficialRegisterRecord` | [`administrativeLedger`](./libro-registro.md) |
| States `PRESENTED` / `RECEIVED_FROM_OTHER_ORG_UNIT` / `TRANSFERRED_FROM_OTHER_ORG_UNIT` | `entryDirection`: `ENTRY` / `EXIT` |

[:octicons-arrow-right-24: Go to Register Ledger (`administrativeLedger`)](./libro-registro.md)

---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
