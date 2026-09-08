# :material-tag: DataTypeRef

## Descripción

Objeto para referenciar a un tipo de dato gestionado por DENA (ej: Expediente, Notificación, Pago...).

!!! info "Al menos uno obligatorio"

    Se debe incluir `oid` **o** `id` (o ambos). Si se incluyen los dos, `oid` tiene prioridad.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `oid` | `OID` | :material-close:* | Identificador interno del tipo de dato (`DN00DataTypeOID`) |
| `id` | `ID` | :material-close:* | Identificador textual del tipo de dato (`DN00DataTypeID`) |

Clase: `DN00DataTypeRef` (`@MarshallType(as="dataTypeRef")`), especialización de `DN00DENAObjectWithIDRefBase`.

---

## Ejemplo

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

---

## Valores del enum `DN00DataTypeEnum`

Los tipos de dato de DATA-RETRIEVE se definen en el enum `DN00DataTypeEnum`. El valor de `id` de cada tipo coincide con el marshallTypeId del objeto de datos correspondiente:

| Valor de enum | `id` (marshallTypeId) | Objeto de dato |
|---|---|---|
| `ADMINISTRATIVE_NOTICE` | `administrativeNotice` | Notificación |
| `ADMINISTRATIVE_RECORD` | `administrativeServiceProcedureRecord` | Expediente |
| `ADMINISTRATIVE_REGISTER` | `administrativeOfficialRegisterRecord` | Registro oficial |
| `PAYMENT_ONE_OFF_PAYMENT` | `oneOffPayment` | Pago único |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | `directDebitPayment` | Domiciliación |
| `SCHEDULE` | `scheduleItem` | Cita |
| `PERSON_DATA` | `personData` | Datos de persona |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
