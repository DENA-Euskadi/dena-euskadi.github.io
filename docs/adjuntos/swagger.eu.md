# :material-api: Swagger Baliabideak (OpenAPI)

DENAren REST zerbitzuen **Swagger UI** interfazeak. Bakoitzetik endpoint-ak, request/response eskemak esploratu eta deiak zuzenean ingurunearen aurka proba ditzakezu.

!!! info "Erreferentziako ingurunea"

    Estekek **PRE** ingurunera apuntatzen dute. Zerbitzu bakoitzak bere OpenAPI espezifikazioa ere argitaratzen du JSON formatuan (`/v3/api-docs`), bezeroak sortzeko edo definizioa beste tresnetan inportatzeko erabilgarria.

---

## Zerbitzuak

| Zerbitzua | Operatiba | Swagger UI (PRE) | OpenAPI (JSON) |
|-----------|-----------|:---------------:|:--------------:|
| **admin-sync-rest** | [Metadata-Sync](../operativas/metadata-sync.md) (SRMD jasotzea) | [:material-api: /sync/swagger-ui](https://api-batera.pre.dena.eus/sync/swagger-ui/index.html) | [:material-code-json: /sync/v3/api-docs](https://api-batera.pre.dena.eus/sync/v3/api-docs) |
| **person-admin-sync-rest** | [Person-Sync](../operativas/person-sync.md) (Pull job-ak eta sinkronizazioa) | [:material-api: /person-sync/swagger-ui](https://api-batera.pre.dena.eus/person-sync/swagger-ui/index.html) | [:material-code-json: /person-sync/v3/api-docs](https://api-batera.pre.dena.eus/person-sync/v3/api-docs) |
| **person-admin-rest** | [Person-Sync](../operativas/person-sync.md) (pertsonen kontsulta: fetch/head/search) | [:material-api: /person/swagger-ui](https://api-batera.pre.dena.eus/person/swagger-ui/index.html) | [:material-code-json: /person/v3/api-docs](https://api-batera.pre.dena.eus/person/v3/api-docs) |

---

!!! tip "Espezifikazio nabigagarria"

    Mezu bakoitzaren eremuz eremuko deskribapen funtzionala [Semantika](../semantica/index.md) atalean dago. [REST mezuen JSON Schema](./json-schema.md) dokumentazio horren osagarri formal eta balidagarria da.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
