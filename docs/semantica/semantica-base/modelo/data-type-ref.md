# :material-tag: DataTypeRef

## Descripción

`dataType` es el campo del `context` que indica **qué tipo de dato** se está solicitando o intercambiando. Es la pieza que tu administración lee para saber qué objeto tiene que devolver.

!!! tip "Lo único que necesita una administración"

    **Basta con leer `dataType.id`**: es un texto (p. ej. `SCHEDULE`) que identifica el tipo de dato. Según su valor, devuelves el objeto correspondiente. El `oid` es un identificador interno de DENA y **no hace falta interpretarlo**.

---

## Atributos JSON

| Campo | Obligatorio | Descripción |
|---|:---:|---|
| `id` | ✅* | Identificador textual del tipo de dato. Es lo que usas para saber qué objeto devolver |
| `oid` | ❌* | Identificador interno de DENA (un GUID). De uso interno; no hace falta interpretarlo |

<small>*Al menos uno de los dos. En la práctica siempre llega el `id`.</small>

---

## Ejemplo

```json
{
    "id": "SCHEDULE",
    "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4006"
}
```

---

## Catálogo de tipos de dato (`dataType.id`)

Valores válidos que puede llegar en `dataType.id` y el objeto de dato que corresponde devolver:

| `dataType.id` | Objeto de dato a devolver | Documentación |
|---|---|---|
| `ADMINISTRATIVE_RECORD` | Expediente administrativo | [expediente.md](../../data-retrieve/data/expediente.md) |
| `ADMINISTRATIVE_NOTICE` | Notificación | [notificacion.md](../../data-retrieve/data/notificacion.md) |
| `PAYMENT_ONE_OFF_PAYMENT` | Pago único | [pago.md](../../data-retrieve/data/pago.md) |
| `PAYMENT_DIRECT_DEBIT_PAYMENT` | Domiciliación bancaria | [pago.md](../../data-retrieve/data/pago.md) |
| `SCHEDULE` | Cita previa | [cita.md](../../data-retrieve/data/cita.md) |
| `PERSON_DATA` | Datos de la persona | [persona.md](../../data-retrieve/data/persona.md) |
| `PERSON_CREDENTIAL` | Credencial de persona | [credencial-persona.md](../../data-retrieve/data/credencial-persona.md) |
| `PERSON_CREDENTIAL_FOR_BASQUE_LANGUAGE_DEGREE` | Credencial de perfil lingüístico de euskera | [credencial-persona.md](../../data-retrieve/data/credencial-persona.md) |
| `ECONOMIC_BENEFIT` | Beneficio económico | [beneficio-economico.md](../../data-retrieve/data/beneficio-economico.md) |
| `ADMINISTRATIVE_LEDGER` | Libro registro | [libro-registro.md](../../data-retrieve/data/libro-registro.md) |
| `ADMINISTRATIVE_PERSON_REGISTER` | Registro de persona | [registro-persona.md](../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_HOUSING_PEV` | Registro de persona — PEV vivienda | [registro-persona.md](../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_LAST_WILL` | Registro de persona — últimas voluntades | [registro-persona.md](../../data-retrieve/data/registro-persona.md) |
| `ADMINISTRATIVE_PERSON_REGISTER_FOR_REVISION_PEV` | Registro de persona — revisión PEV | [registro-persona.md](../../data-retrieve/data/registro-persona.md) |
| `PERSON_ENROLLMENT` | Matrícula | [matricula.md](../../data-retrieve/data/matricula.md) |
| `PERSON_ACADEMIC_ENROLLMENT` | Matrícula académica | [matricula.md](../../data-retrieve/data/matricula.md) |
| `PERSON_MEDICAL_DATA` | Datos médicos | [datos-medicos.md](../../data-retrieve/data/datos-medicos.md) |

> ⚠️ El tipo `ADMINISTRATIVE_OFFICIAL_REGISTER_RECORD` está **obsoleto** y ha sido eliminado del catálogo. Su sustituto es `ADMINISTRATIVE_LEDGER`.

<!-- DENA-DOC-FOOTER -->
