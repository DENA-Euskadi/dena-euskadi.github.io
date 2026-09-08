# :material-check-circle: Status (Respuesta)

## Descripción

Los mensajes de respuesta (`DN00InteropResponseMessageBase`) incluyen información sobre el resultado del procesamiento mediante tres campos: `code`, `errorId` y `details`.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `code` | `DN00InteropResponseStatus` | :material-check: | Código del estado de procesamiento del mensaje |
| `errorId` | `DN00InteropResponseStatusCode` | :material-close: | Código de error específico (presente solo en errores) |
| `details` | `DN00InteropResponseStatusDetails` | :material-close: | Detalles de la respuesta (un único campo `details` de tipo texto) |

---

## DN00InteropResponseStatus (campo `code`)

| Valor | Descripción |
|---|---|
| `OK` | Mensaje procesado correctamente |
| `CLIENT_ERR` | Error del cliente (ej: datos erróneos) |
| `SERVER_ERR` | Error en el servidor |
| `QUEUED` | El mensaje ha sido puesto en cola de procesamiento asíncrono |

---

## Ejemplos

**Respuesta correcta:**

```json
{
  "code": "OK",
  "errorId": null,
  "details": null
}
```

**Respuesta con error de cliente:**

```json
{
  "code": "CLIENT_ERR",
  "errorId": "ENTITY_NOT_FOUND",
  "details": {
    "details": "La persona solicitada no existe en el sistema"
  }
}
```

**Respuesta con error de servidor:**

```json
{
  "code": "SERVER_ERR",
  "errorId": "UNEXPECTED_ERROR",
  "details": {
    "details": "Connection timeout accessing database"
  }
}
```

!!! note "Estructura de `details`"
    `DN00InteropResponseStatusDetails` es una clase con un único campo de texto (`details`). No hay subtipos específicos por código de error en el modelo actual: el detalle del error se transmite como texto libre.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
