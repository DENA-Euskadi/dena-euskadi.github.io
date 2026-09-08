# :material-check-circle: Status (Respuesta)

## Descripción

La estructura `status` está presente **únicamente en mensajes de respuesta**. Incluye información sobre el resultado del procesamiento: si fue exitoso, si hubo un error, o si se procesará de forma asíncrona.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `statusCode` | `DENAResponseStatusCode` | :material-check: | Código del estado de procesamiento del mensaje |
| `statusDetails` | `Object` | :material-close: | Detalles de la respuesta. El tipo depende de `statusCode` |

---

## DENAResponseStatusCode

| Valor | Descripción |
|---|---|
| `OK` | Mensaje procesado correctamente |
| `CLIENT_ERR` | Error del cliente (ej: datos erróneos) |
| `SERVER_ERR` | Error en el servidor |
| `QUEUED` | El mensaje ha sido puesto en cola de procesamiento asíncrono |

---

## Status Details por código

| statusCode | Tipo de statusDetails |
|---|---|
| `OK` | *(vacío)* |
| `CLIENT_ERR` | [DENAClientErrDetails](#denaclienterrdetails) |
| `SERVER_ERR` | [DENAServerErrDetails](#denaservererrdetails) |
| `QUEUED` | [DENAAsyncQueueData](#denaasyncqueuedata) |

---

## DENAClientErrDetails

Detalles del error cuando `statusCode = CLIENT_ERR`.

| Campo | Tipo | Descripción |
|---|---|---|
| `errorCode` | `ID` | Identificador del error. Ej: `ENTITY_NOT_FOUND` |
| `about` | `String` | Datos de la entidad relacionada con el error (ej: personOid/personId) |
| `errorDetails` | `String` | Detalles adicionales del error |

**Ejemplo:**

```json
{
  "status": {
    "statusCode": "CLIENT_ERR",
    "statusDetails": {
      "errorCode": "ENTITY_NOT_FOUND",
      "about": "12345678A",
      "errorDetails": "La persona solicitada no existe en el sistema"
    }
  }
}
```

---

## DENAServerErrDetails

Detalles del error cuando `statusCode = SERVER_ERR`.

| Campo | Tipo | Descripción |
|---|---|---|
| `errorCode` | `ID` | Identificador del error. Ej: `UNEXPECTED_ERROR` |
| `errorDetails` | `String` | Detalles del error (ej: stack trace) |

**Ejemplo:**

```json
{
  "status": {
    "statusCode": "SERVER_ERR",
    "statusDetails": {
      "errorCode": "UNEXPECTED_ERROR",
      "errorDetails": "Connection timeout accessing database"
    }
  }
}
```

---

## DENAAsyncQueueData

Detalles cuando `statusCode = QUEUED` (procesamiento asíncrono).

| Campo | Tipo | Descripción |
|---|---|---|
| `jobToken` | `Token` | Token asignado al job planificado, permite consultar su estado |
| `jobStatusQueryUrl` | `URL` | URL para consultar el estado del job |

**Ejemplo:**

```json
{
  "status": {
    "statusCode": "QUEUED",
    "statusDetails": {
      "jobToken": "a3f2c891-4b5d-4e6f-8a9b-1c2d3e4f5a6b",
      "jobStatusQueryUrl": "https://interop.api.dena.eus/queued-jobs/a3f2c891-4b5d-4e6f-8a9b-1c2d3e4f5a6b"
    }
  }
}
```

---

## Consulta de estado de un Job asíncrono

Al consultar `jobStatusQueryUrl` se obtiene:

| Campo | Tipo | Descripción |
|---|---|---|
| `jobToken` | `Token` | Token del job consultado |
| `jobStatus` | `Enum` | Estado actual del job |

**Valores de `jobStatus`:**

| Valor | Descripción |
|---|---|
| `QUEUED` | En cola, pendiente de ejecución |
| `EXECUTING` | En proceso de ejecución |
| `FINISHED` | Terminado correctamente |
| `FAILED` | Terminado con error |
| `DISCARDED` | Descartado |

---

## Procesamiento asíncrono

!!! info "Flujo asíncrono"
    En invocaciones asíncronas, el resultado NO se incluye en el mensaje de respuesta inmediata. Se devuelve un token y el mensaje se pone en cola. Cuando termina el procesamiento, hay dos opciones:

    1. **Callback**: el servidor avisa al origen en la URL de callback proporcionada en la sección `protocol`
    2. **Polling**: el origen consulta periódicamente el estado usando `jobStatusQueryUrl`

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
