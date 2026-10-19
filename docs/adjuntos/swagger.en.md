# :material-api: Swagger Resources (OpenAPI)

**Swagger UI** interfaces of the DENA REST services. From each one you can explore the endpoints, the request/response schemas and try out the calls directly against the environment.

!!! info "Reference environment"

    The links point to the **PRE** environment. Each service also publishes its OpenAPI specification in JSON format (`/v3/api-docs`), useful to generate clients or import the definition into other tools.

---

## Services

| Service | Operation | Swagger UI (PRE) | OpenAPI (JSON) |
|---------|-----------|:---------------:|:--------------:|
| **admin-sync-rest** | [Metadata-Sync](../operativas/metadata-sync.md) (SRMD reception) | [:material-api: /sync/swagger-ui](https://api-batera.pre.dena.eus/sync/swagger-ui/index.html) | [:material-code-json: /sync/v3/api-docs](https://api-batera.pre.dena.eus/sync/v3/api-docs) |
| **person-admin-sync-rest** | [Person-Sync](../operativas/person-sync.md) (Pull jobs and synchronization) | [:material-api: /person-sync/swagger-ui](https://api-batera.pre.dena.eus/person-sync/swagger-ui/index.html) | [:material-code-json: /person-sync/v3/api-docs](https://api-batera.pre.dena.eus/person-sync/v3/api-docs) |
| **person-admin-rest** | [Person-Sync](../operativas/person-sync.md) (person query: fetch/head/search) | [:material-api: /person/swagger-ui](https://api-batera.pre.dena.eus/person/swagger-ui/index.html) | [:material-code-json: /person/v3/api-docs](https://api-batera.pre.dena.eus/person/v3/api-docs) |

---

!!! tip "Browsable specification"

    The functional, field-by-field description of each message lives in the [Semantics](../semantica/index.md) section. The [REST messages JSON Schema](./json-schema.md) is the formal, validatable complement to that documentation.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
