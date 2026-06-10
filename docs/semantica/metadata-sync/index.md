# METADATA-SYNC

## ¿Qué es?

**Metadata-Sync** es el mecanismo mediante el cual las administraciones notifican a DENA cuando se producen cambios en algún dato asociado a una persona usuaria de DENA.

DENA solo almacenará los metadatos de actualización, consistentes en la fecha de ultima actualización para cada combinación de persona, tipo de dato y administración.

Gracias a estos metadatos, la aplicación cliente podra saber cuando ha de realizar una nueva consulta a cada administración (a traves de DENA) para sincronizar los ultimos cambios que se hayan producido en la datos de la persona usuaria.

---

## Documentación

### Endpoint

| Documento | Contenido |
|-----------|-----------|
| [endpoint-sync-metadata.md](./endpoint-sync-metadata.md) | Endpoint para la notificación de cambios en datos asociados a personas |

### Postman

Colección y environment Postman en [`docs/adjuntos/postman/`](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos/postman).

### Documentación Swagger

<!-- Enlace al Swagger / OpenAPI -->

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.1.0 · 2025-02-28</sub>
