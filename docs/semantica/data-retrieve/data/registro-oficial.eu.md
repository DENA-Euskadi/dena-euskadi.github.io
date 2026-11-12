!!! danger "⚠️ MOTA ZAHARKITUA — Erabili Erregistro Liburua haren ordez"
    `DN00AdministrativeOfficialRegisterRecord` eta `ADMINISTRATIVE_REGISTER` mota **zaharkituta** daude: **ez daude** v0.5.20-ko `DN00DataTypeEnum` katalogoan eta **ez dute Java inplementaziorik** iturburu-kodean. Ez erabili mota hau inplementazio berrietan; `"type": "administrativeOfficialRegisterRecord"` balioarekin bidalitako edozein objektu baztertuko da.

    **Erregistroko sarrerak/irteerak dokumentatzen dituen mota [`libro-registro.md`](./libro-registro.md) da (`AdministrativeLedger` / `administrativeLedger`).**

# :material-book-open-page-variant: Erregistro Ofiziala (Official Register Record) — ZAHARKITUA

> - **Egoera:** ❌ Zaharkitua / v{{ dena.version }}-n inplementatu gabe
> - **Ordezkoa:** [Erregistro Liburua](./libro-registro.md) (`administrativeLedger`)

## Zergatik dago zaharkitua?

Mota honek espediente bati lotutako sarrera/irteera erregistroko idazpena dokumentatzen zuen.
v0.5.20-n haren funtzionaltasuna **Erregistro Liburuak** (`administrativeLedger`) estaltzen du,
erregistroko sarrerak eta irteerak `entryDirection` eremuaren bidez modelatzen baititu
(`ENTRY` / `EXIT`).

`administrativeOfficialRegisterRecord` mota:

- **Ez dago** `DN00DataTypeEnum` katalogoan (ikusi [DataTypeRef](../../semantica-base/modelo/data-type-ref.md)).
- **Ez du** Java klaserik iturburu-kodean (`DN00AdministrativeOfficialRegisterRecord` eta bere
  egoerak ez dira iturburu gisa existitzen).

## Zer erabili haren ordez

| Lehen (zaharkitua) | Orain (v0.5.20) |
|---|---|
| `administrativeOfficialRegisterRecord` | [`administrativeLedger`](./libro-registro.md) |
| `PRESENTED` / `RECEIVED_FROM_OTHER_ORG_UNIT` / `TRANSFERRED_FROM_OTHER_ORG_UNIT` egoerak | `entryDirection`: `ENTRY` / `EXIT` |

[:octicons-arrow-right-24: Joan Erregistro Liburura (`administrativeLedger`)](./libro-registro.md)

---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
