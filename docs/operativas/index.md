# :material-cogs: Operativas

Esta sección describe las **operaciones que tu administración puede implementar** para integrarse con DENA. Cada operativa se explica primero conceptualmente y luego con detalle técnico de implementación.

---

## Flujo recomendado

1. **Empieza por Data-Retrieve** — Es la operativa fundamental. Sin ella, DENA no puede mostrar datos de tu admin.
2. **Luego Metadata-Sync** — Para que DENA sepa cuándo hay datos nuevos sin tener que consultar constantemente.
3. **Finalmente Person-Sync** — Para saber qué personas están en DENA y enviar avisos solo para ellas.

---

## Operativas disponibles

<div class="grid cards" markdown>

-   :material-database-arrow-right:{ .lg .middle } **Data-Retrieve (Servir Datos)**

    ---

    DENA llama a tu admin para obtener los datos de una persona.

    [:octicons-arrow-right-24: Ver detalle](./data-retrieve.md)

-   :material-bell-ring:{ .lg .middle } **Metadata-Sync (Notificar Cambios)**

    ---

    Tu admin envía a DENA avisos de que hay datos nuevos o actualizados.

    [:octicons-arrow-right-24: Ver detalle](./metadata-sync.md)

-   :material-account-sync:{ .lg .middle } **Person-Sync (Sincronizar Personas)**

    ---

    Mantener sincronizado el listado de personas inscritas en DENA.

    [:octicons-arrow-right-24: Ver detalle](./person-sync.md)

</div>

---

**Siguiente:** [:octicons-arrow-right-24: Semántica (especificación técnica)](../semantica/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
