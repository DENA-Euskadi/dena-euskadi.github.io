# :material-tag: DataTypeRef

## Descripción

`dataType` es el campo del `context` que indica **qué tipo de dato** se está solicitando o intercambiando (Expediente, Notificación, Pago...). Es la pieza que una administración lee para saber qué objeto tiene que devolver.

!!! tip "Lo único que necesita una administración"

    Para implementar el endpoint, **basta con leer `dataType.id`**: es un texto del catálogo (p. ej. `administrativeServiceProcedureRecord`) que identifica el tipo de dato. Según su valor, devuelves el objeto correspondiente. El `oid` es un identificador interno de DENA y **no hace falta interpretarlo**.

---

## Las tres piezas (y por qué existen)

El modelo separa tres conceptos que a menudo se confunden. En la práctica solo trabajarás con el `id`:

| Pieza | Clase Java | Qué es | Ejemplo |
|---|---|---|---|
| **`id`** | `DN00DataTypeID` (`@MarshallType(as="dataTypeId")`) | Identificador **textual** del tipo de dato. Es el valor del catálogo y coincide con el `marshallTypeId` del objeto de datos. **Es lo que interpretas.** | `"administrativeServiceProcedureRecord"` |
| **`oid`** | `DN00DataTypeOID` (`@MarshallType(as="dataTypeOid")`) | Identificador **interno** de DENA (un GUID). De uso interno; una administración no necesita usarlo. | `"6AE83A0C-2202-4666-9857-3334C14663A2"` |
| **`dataType`** (contenedor) | `DN00DataTypeRef` (`@MarshallType(as="dataTypeRef")`) | El objeto que agrupa `oid` + `id` y viaja dentro del `context`. Especializa `DN00DENAObjectWithIDRefBase`. | `{ "id": "...", "oid": "..." }` |

!!! info "¿`oid` o `id`?"

    Se debe incluir `oid` **o** `id` (o ambos). En DATA-RETRIEVE, DENA siempre envía el `id`, que es el que debes usar. Si vinieran los dos, el `oid` tiene prioridad a nivel interno, pero el `id` siempre es suficiente para decidir qué objeto devolver.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `id` | `String` | :material-check:* | Identificador textual del tipo de dato (`DN00DataTypeID`). Uno de los valores del catálogo (ver tabla abajo) |
| `oid` | `String` | :material-close:* | Identificador interno de DENA (`DN00DataTypeOID`, un GUID). De uso interno |

<small>*Al menos uno de los dos. En la práctica siempre llega el `id`.</small>

---

## Ejemplo

```json
{
    "id": "administrativeServiceProcedureRecord",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

> El `id` es el que usas; el `oid` (un GUID interno) puede venir o no y no necesitas interpretarlo.

---

## Catálogo de tipos de dato (`id`)

Los valores válidos de `id` están definidos en el enum `DN00DataTypeEnum`. Cada valor coincide con el `marshallTypeId` del objeto de datos correspondiente, de modo que el `id` te dice directamente qué objeto devolver:

| `id` (valor de `dataType.id`) | Objeto de dato a devolver | Constante del enum `DN00DataTypeEnum` |
|---|---|---|
| `administrativeServiceProcedureRecord` | Expediente | `ADMINISTRATIVE_RECORD` |
| `administrativeNotice` | Notificación | `ADMINISTRATIVE_NOTICE` |
| `administrativeOfficialRegisterRecord` | Registro oficial | `ADMINISTRATIVE_REGISTER` |
| `oneOffPayment` | Pago único | `PAYMENT_ONE_OFF_PAYMENT` |
| `directDebitPayment` | Domiciliación | `PAYMENT_DIRECT_DEBIT_PAYMENT` |
| `scheduleItem` | Cita | `SCHEDULE` |
| `personData` | Datos de persona | `PERSON_DATA` |

> El enum `DN00DataTypeEnum` vive en `dena-common-data-api`; los identificadores (`DN00DataTypeID`/`DN00DataTypeOID`) y el contenedor `DN00DataTypeRef` viven en `dena-common-api`.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
