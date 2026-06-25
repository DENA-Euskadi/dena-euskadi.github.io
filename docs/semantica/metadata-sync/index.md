# :material-sync: METADATA-SYNC

> **Versión:** `v{{ dena.version }}` · **Fecha:** {{ dena.date }}

---

## ¿Qué es?

**Metadata-Sync** es el mecanismo mediante el cual las administraciones notifican a DENA cuando se producen cambios en algún dato asociado a una persona usuaria.

``` mermaid
sequenceDiagram
    participant Admin as Administración
    participant DENA as CORE DENA
    participant App as App Cliente

    Admin->>DENA: POST /syncMetadata (persona X tiene cambios)
    DENA-->>Admin: 200 OK

    Note over DENA: Almacena metadato: persona + tipo + fecha

    App->>DENA: ¿Hay novedades?
    DENA-->>App: Sí, Admin X tiene datos nuevos para ti
```

!!! info "Solo metadatos"

    DENA **no almacena los datos en sí**, solo la fecha de última actualización por combinación de:

    - Persona
    - Tipo de dato
    - Administración

    Cuando la app cliente necesite los datos reales, los pedirá vía [Data-Retrieve](../data-retrieve/index.md).

---

## Documentación

| Documento | Contenido |
|---|---|
| [:octicons-arrow-right-24: Endpoint](./endpoint-sync-metadata.md) | Contrato REST para la notificación de cambios |

---

!!! tip "Postman"

    Colección y environment Postman disponibles en [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
