# :material-code-json: JSON Schema de los mensajes REST

Esquema **JSON Schema (draft 2020-12)** que describe la estructura de todos los mensajes REST de interoperabilidad de DENA y de los modelos de datos que intercambian (expedientes, notificaciones, pagos, personas, citas, etc.).

!!! info "¿Para qué sirve?"

    Este esquema es una referencia **generada a partir de los modelos reales** del CORE de DENA. Puedes usarlo para:

    - **Validar** los mensajes que tu administración envía o recibe (request/response) antes de integrarlos.
    - **Generar tipos / clientes** en tu lenguaje (TypeScript, Java, Python…) a partir del esquema.
    - **Consultar** de un vistazo los campos, tipos y valores de enumerado admitidos.

---

## Acceso al esquema

El esquema se sirve directamente desde el entorno **PRE**, siempre actualizado a partir de los modelos reales del CORE:

| Recurso | Descripción | Acceso |
|---------|-------------|:--------:|
| **DENA REST Messages — JSON Schema** | Esquema de todos los mensajes REST (data-retrieve, metadata-sync, person-sync, person fetch/head/search) y de los modelos de datos intercambiados. | [:material-download: Descargar](https://api-batera.pre.dena.eus/jsonschema-utils/json-schema?download=true) · [:material-eye: Ver online](https://api-batera.pre.dena.eus/jsonschema-utils/json-schema?download=false) |

!!! info "Descarga vs. visualización"
    - `?download=true` entrega el esquema como **fichero** descargable.
    - `?download=false` lo muestra **directamente** en el navegador.

---

## Mensajes incluidos

El esquema describe, entre otros, estos mensajes de nivel superior:

| Operativa | Mensaje (request / response) |
|-----------|------------------------------|
| **Data-Retrieve** | `DN00COREToConnectorDataRetrieveRequestMessage` / `DN00COREToConnectorDataRetrieveResponseMessage` |
| **Metadata-Sync (SRMD)** | `DN00SyncMetaDataFromAdminRequestMessage` / `DN00SyncMetaDataFromAdminResponseMessage` |
| **Person-Sync (push)** | `DN00PersonInteropPushToAdminNotificationMessage` |
| **Person-Sync (pull bespoke)** | `DN00PersonInteropPullFromAdminBespokeJobCreate…` / `…BespokeJobGet…` / `…BespokeExportAssetFetch…` |
| **Person-Sync (pull pregen)** | `DN00PersonInteropPullFromAdminPreGenJobGet…` / `…PreGenExportAssetFetch…` |
| **Persona (fetch / head / search)** | `DN00PersonFetchInterop…` / `DN00PersonHeadInterop…` / `DN00PersonInteropSearch…` |

Cada objeto de dato intercambiado lleva un **discriminador de tipo** (campo `type`), por ejemplo `administrativeServiceProcedureRecord`, `administrativeNotice`, `payment`, `directDebitPayment`, `personData` o `scheduleItem`.

---

!!! tip "Semántica detallada"

    La descripción funcional campo a campo de cada mensaje y modelo está en la sección [Semántica](../semantica/index.md). Este esquema es el complemento **formal y validable** de esa documentación.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
