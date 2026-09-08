# :material-message-text: Tipos de Mensaje

Esta página documenta los tipos de dato utilizados internamente en la estructura de los mensajes de interoperabilidad DENA.

---

## DenaFlowDirection

Enum que indica la dirección del mensaje en la conversación.

| Valor | Descripción |
|---|---|
| `REQ` | Petición (request) |
| `RES` | Respuesta a una petición (response) |

---

## DenaMessageType

Enum que identifica el tipo de operación de interoperabilidad. Cada flujo tiene sus propios tipos de mensaje:

| Flujo | Origen | Destino | Valor | Descripción |
|---|---|---|---|---|
| Person-Sync | DENA-CORE | Admin | `DENA_USER_SYNC_PUSH` | Notificación de alta/baja de personas |
| Metadata-Sync | Client | DENA-CORE | `UI_META_DATA_SYNC` | Sincronización de metadatos desde la UI |
| Metadata-Sync | Admin | DENA-CORE | `ADMIN_SYNC_PUSH` | Envío de cambios desde la administración (PUSH) |
| Metadata-Sync | DENA-CORE | Admin | `DENA_SYNC_PULL` | Solicitud de cambios a la administración (PULL) |
| Data-Retrieve | Client | DENA-CORE | `UI_DATA_RETRIEVE` | Solicitud de datos desde la UI |
| Data-Retrieve | DENA-CORE | Admin | `DENA_DATA_RETRIEVE` | Solicitud de datos a la administración |

---

## DenaInteropRouteDataItem

Cada vez que un mensaje de interoperabilidad pasa por un componente de DENA, este **deja una traza** en el array `interopRouteData`. Esta información se usa principalmente para auditoría y depuración.

### Atributos

| Campo | Tipo | Descripción |
|---|---|---|
| `denaComponentId` | `ID` | Identificador del componente DENA |
| `timeStamp` | `TimeStamp` | Instante en el que el mensaje pasó por el componente |

### Identificadores de componentes conocidos

| ID | Componente |
|---|---|
| `mobileApp` | Aplicación móvil DENA |
| `webApp` | Aplicación web DENA |
| `apiGateway` | API Gateway |
| `denaCORE` | DENA-CORE (módulo central) |
| `connector` | Conector de administración |

### Ejemplo

```json
{
  "interopRouteData": [
    {
      "denaComponentId": "mobileApp",
      "timeStamp": 1670374400
    },
    {
      "denaComponentId": "denaCORE",
      "timeStamp": 1670374401
    },
    {
      "denaComponentId": "connector",
      "timeStamp": 1670374402
    }
  ]
}
```

---

## DenaPersonAndConsentGiven

Estructura que combina datos mínimos de una persona con datos de un consentimiento otorgado.

| Campo | Tipo | Descripción |
|---|---|---|
| `personRef` | [DenaPersonRef](./person-ref.md) | Datos mínimos de la persona (oid, dni, fecha alta, etc.) |
| `consentRef` | `DenaConsentRef` | Referencia de un consentimiento (oid, url, etc.) |

### Ejemplo

```json
{
  "personRef": {
    "personId": "12345678A",
    "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2"
  },
  "consentRef": {
    "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentURL": "https://interop.api.dena.eus/consent/db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
  }
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
