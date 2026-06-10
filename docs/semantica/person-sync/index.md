# PERSON-SYNC

## ¿Qué es?

**Person-Sync** es el que mecanismo que permite a las administraciones sincronizar las personas registradas en DENA en sus sistemas y mantenerlos actualizados, para poder notificar a DENA las actualizaciones en los datos asociados a esas personas.

Para ello se ofrecen dos mecanismos:
 - [Pull](./pull.md): La administración se encargará de conectar con DENA y descargarse los datos de los usuarios. Se generarán periodicamente ficheros con los cambios incrementales para un periodo determinado de tiempo. Ademas, en caso de ser necesario, se ofrece la posibilidad de solicitar la generación de ficheros a medida, indicando los filtros a aplicar, como el horizonte temporal que se requiere.
 - [Push](./push.md): En caso de que la administración provea un endpoint para la notificación de cambios, se podrá configurar en DENA el envío de estos en el momento que se produzcan.

---

## Documentación

### Endpoints Pull

| Documento | Contenido |
|-----------|-----------|
| [Fetch Persons Pregen Export Asset](./endpoints/pull/fetch-persons-pregen-export-asset.md) | Endpoint de descarga de ficheros pregenerados |
| [Create Pull from Admin Bespoke Job](./endpoints/pull/create-pull-from-admin-bespoke-job.md) | Endpoint de creación de solicitud de exportación a medida |
| [Get Pull from Admin Bespoke Job](./endpoints/pull/get-pull-from-admin-bespoke-job.md) | Endpoint de consulta de estado de solicitudes de exportación a medida |
| [Fetch Persons Bespoke Export Asset](./endpoints/pull/fetch-persons-bespoke-export-asset.md) | Endpoint de descarga de ficheros generados por solicitudes de exportación a medida |

### Endpoints Push

| Documento | Contenido |
|-----------|-----------|
| [Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) | Contrato del endpoint de envío de actualizaciones: request, response, ejemplos JSON y códigos HTTP |

### Postman

Colección y environment Postman en [`docs/adjuntos/postman/`](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos/postman).

### Documentación Swagger

<!-- Enlace al Swagger / OpenAPI -->


<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
