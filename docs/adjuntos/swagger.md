# :material-api: Recursos Swagger (OpenAPI)

Interfaces **Swagger UI** de los servicios REST de DENA. Desde cada una puedes explorar los endpoints, los esquemas de request/response y probar las llamadas directamente contra el entorno.

!!! info "Entorno de referencia"

    Los enlaces apuntan al entorno **PRE**. Cada servicio publica también su especificación OpenAPI en formato JSON (`/v3/api-docs`), útil para generar clientes o importar la definición en otras herramientas.

---

## Servicios

| Servicio | Operativa | Swagger UI (PRE) | OpenAPI (JSON) |
|----------|-----------|:---------------:|:--------------:|
| **admin-sync-rest** | [Metadata-Sync](../operativas/metadata-sync.md) (recepción de SRMD) | [:material-api: /sync/swagger-ui](https://api-batera.pre.dena.eus/sync/swagger-ui/index.html) | [:material-code-json: /sync/v3/api-docs](https://api-batera.pre.dena.eus/sync/v3/api-docs) |
| **person-admin-sync-rest** | [Person-Sync](../operativas/person-sync.md) (Pull jobs y sincronización) | [:material-api: /person-sync/swagger-ui](https://api-batera.pre.dena.eus/person-sync/swagger-ui/index.html) | [:material-code-json: /person-sync/v3/api-docs](https://api-batera.pre.dena.eus/person-sync/v3/api-docs) |
| **person-admin-rest** | [Person-Sync](../operativas/person-sync.md) (consulta de personas: fetch/head/search) | [:material-api: /person/swagger-ui](https://api-batera.pre.dena.eus/person/swagger-ui/index.html) | [:material-code-json: /person/v3/api-docs](https://api-batera.pre.dena.eus/person/v3/api-docs) |

---

!!! tip "Especificación navegable"

    La descripción funcional campo a campo de cada mensaje está en la sección [Semántica](../semantica/index.md). El [JSON Schema de los mensajes REST](./json-schema.md) es el complemento formal y validable de esa documentación.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
