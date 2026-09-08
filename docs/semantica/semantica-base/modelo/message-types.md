# :material-message-text: Tipos de Mensaje

Esta página documenta los tipos usados en la estructura del **interop message** de DENA (`DN00InteropContext` y `DN00InteropMessageData`).

---

## DN00InteropFlowDirection

Enum que indica la dirección del mensaje. No se almacena de forma independiente: se deriva del tipo de mensaje.

| Valor | Descripción |
|---|---|
| `REQUEST` | Petición |
| `RESPONSE` | Respuesta a una petición |

---

## DN00InteropMessageType

Enum que identifica el tipo de operación de interoperabilidad. Estos son los valores reales del enum:

| Valor | Ámbito |
|---|---|
| `CLIENT_LOGIN`, `CLIENT_LOGIN_DEMO` | Login del cliente |
| `CLIENT_PASSKEY_REGISTER_INIT` / `CLIENT_PASSKEY_REGISTER_FINISH` | Registro de passkey |
| `CLIENT_PASSKEY_LOGIN_INIT` / `CLIENT_PASSKEY_LOGIN_FINISH` | Login con passkey |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS` | Limpieza de credenciales passkey |
| `CLIENT_INIT_REQ` / `CLIENT_INIT_RESP` | Inicialización del cliente |
| `CLIENT_SRMD_SYNC_REQ` / `CLIENT_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) desde el cliente |
| `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) desde la administración |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Data-Retrieve desde el cliente |
| `PERSON_FETCH_DATA` | Recuperación de datos de una persona |
| `ADMIN_PERSON_PULL_BESPOKE_CREATE_REQ` | Person-Sync Pull: crear job bespoke |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH` | Person-Sync Pull: consultar job bespoke |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` | Descarga de asset bespoke |
| `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH` | Descarga de asset pregenerado |
| `PERSON_ADMIN_SEARCH` | Búsqueda de personas por la administración |
| `PERSON_ADMIN_HEAD` | Consulta HEAD de persona por la administración |

---

## DN00IteropRouteDataItem

Cada vez que un mensaje pasa por un componente de DENA, este **deja una traza** en el array `interopRouteData`. Se usa para auditoría y depuración.

### Atributos

| Campo | Tipo | Descripción |
|---|---|---|
| `denaComponentId` | `DN00InteropComponent` | Identificador del componente DENA |
| `timestamp` | `Instant` (ISO 8601) | Instante en el que el mensaje pasó por el componente |

### Identificadores de componentes

| Valor | Componente |
|---|---|
| `CLIENT_INSTALLMENT` | Instalación cliente (DENA-APP) |
| `DENA_CORE` | DENA-CORE (módulo central) |
| `DENA_ADMIN_CONNECTOR` | Conector de administración |
| `ADMIN` | Sistema de la administración |

### Ejemplo

```json
{
  "interopRouteData": [
    {
      "denaComponentId": "CLIENT_INSTALLMENT",
      "timestamp": "2026-08-18T11:28:47.523Z"
    },
    {
      "denaComponentId": "DENA_CORE",
      "timestamp": "2026-08-18T11:28:47.601Z"
    },
    {
      "denaComponentId": "DENA_ADMIN_CONNECTOR",
      "timestamp": "2026-08-18T11:28:47.688Z"
    }
  ]
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
