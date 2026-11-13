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

Enum que identifica el tipo de operación de interoperabilidad. Lista completa de los valores del enum `DN00InteropMessageType` con su dirección y partes implicadas (fuente: `DN00InteropMessageType.java`):

| Petición (`_REQ`) / Respuesta (`_RESP`) | Operación | Origen → Destino |
|---|---|---|
| `CLIENT_LOGIN_DEMO` | Login de demostración del cliente (sin respuesta pareada) | CLIENT_INSTALLMENT → DENA_CORE |
| `CLIENT_LOGIN_REQ` / `CLIENT_LOGIN_RESP` | Login del cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_REFRESH_TOKEN_REQ` / `CLIENT_REFRESH_TOKEN_RESP` | Refresco de token | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS_REQ` / `CLIENT_PASSKEY_CLEAN_CREDENTIALS_RESP` | Limpieza de credenciales passkey | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_LOGIN_INIT_REQ` / `CLIENT_PASSKEY_LOGIN_INIT_RESP` | Login con passkey — inicio | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_LOGIN_FINISH_REQ` / `CLIENT_PASSKEY_LOGIN_FINISH_RESP` | Login con passkey — fin | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_REGISTER_INIT_REQ` / `CLIENT_PASSKEY_REGISTER_INIT_RESP` | Registro de passkey — inicio | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_REGISTER_FINISH_REQ` / `CLIENT_PASSKEY_REGISTER_FINISH_RESP` | Registro de passkey — fin | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_LOGIN_REQ` / `ADMIN_LOGIN_RESP` | Login de la administración | ADMIN ↔ DENA_CORE |
| `CLIENT_INIT_REQ` / `CLIENT_INIT_RESP` | Inicialización del cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_SRMD_SYNC_REQ` / `CLIENT_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) iniciado desde el cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) iniciado desde la administración | ADMIN ↔ DENA_CORE |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Data-Retrieve: recuperación de datos desde el cliente | CLIENT_INSTALLMENT ↔ ADMIN |
| `CLIENT_USER_DELETE_REQ` / `CLIENT_USER_DELETE_RESP` | Borrado de usuario desde el cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_USER_DELETE_BY_ID_REQ` / `CLIENT_USER_DELETE_BY_ID_RESP` | Borrado de usuario por ID desde el cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `PERSON_ADMIN_SEARCH_REQ` / `PERSON_ADMIN_SEARCH_RESP` | Búsqueda de personas por la administración | ADMIN ↔ DENA_CORE |
| `PERSON_FETCH_DATA_REQ` / `PERSON_FETCH_DATA_RESP` | Recuperación de datos de una persona | ADMIN ↔ DENA_CORE |
| `PERSON_ADMIN_HEAD_REQ` / `PERSON_ADMIN_HEAD_RESP` | Consulta HEAD de persona por la administración | ADMIN ↔ DENA_CORE |
| `PERSON_PUSH_TO_ADMIN_REQ` / `PERSON_PUSH_TO_ADMIN_RESP` | Person-Sync Push: DENA envía datos de persona a la administración | DENA_CORE ↔ ADMIN |
| `CLIENT_PERSON_FETCH_REQ` / `CLIENT_PERSON_FETCH_RESP` | Consulta de datos de persona desde el dispositivo cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PERSON_UPDATE_REQ` / `CLIENT_PERSON_UPDATE_RESP` | Actualización de datos de persona desde el dispositivo cliente | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_BESPOKE_CREATE_REQ` / `ADMIN_PERSON_PULL_BESPOKE_CREATE_RESP` | Person-Sync Pull: crear job bespoke | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH_REQ` / `ADMIN_PERSON_PULL_BESPOKE_FETCH_RESP` | Person-Sync Pull: consultar resultado de job bespoke | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_PREGEN_FETCH_REQ` / `ADMIN_PERSON_PULL_PREGEN_FETCH_RESP` | Person-Sync Pull: consultar resultado de job pregenerado | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_RESP` | Person-Sync Pull: descarga de asset bespoke | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_RESP` | Person-Sync Pull: descarga de asset pregenerado | ADMIN ↔ DENA_CORE |

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
| `DENA_INTEROP_ADMIN_SYNC` | Componente de sincronización interop de administración |
| `ADMIN` | Sistema de la administración |
| `DENA_PERSON_ADMIN` | Módulo de administración de personas de DENA |
| `DENA_PERSON_ADMIN_SYNC` | Servicio de sincronización de personas de DENA |
| `DENA_SANDBOX_ADMIN` | Administración sandbox de DENA (entornos de prueba) |

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
