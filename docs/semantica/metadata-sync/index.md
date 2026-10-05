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

    Admin->>DENA: POST /api/admin/interop/sync/metadata (persona X tiene cambios)
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

## Cómo detectar los cambios en tu administración

Lo primero que tiene que hacer un origen de datos para integrarse es **detectar qué ha cambiado**. Para DENA, la única información necesaria es:

- **Qué persona** tiene algún dato modificado (por tipo de dato).
- **Cuándo** fue el último cambio.

El **dato concreto** que ha cambiado **no importa**: solo importa el hecho de que ha habido un cambio en el origen de datos y en qué momento. Un alta o un borrado de fila también cuenta como cambio.

Normalmente esto es una simple consulta SQL por cada tipo de dato. Por ejemplo, para una tabla de negocio con esta estructura:

| PERSON_ID | PERSON_DATA | CREATED_AT | LAST_UPDATED_AT |
|-----------|-------------|------------|-----------------|
| 48291038Z | … | 2026-08-17T03:22:10Z | 2026-08-17T09:14:22Z |
| 10593847H | … | 2026-08-17T14:05:49Z | 2026-08-17T18:41:03Z |

la consulta que devuelve las personas con cambios posteriores a una fecha es directa:

```sql
SELECT PERSON_ID,
       MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE_AT
  FROM DB_TABLE
 WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= :from
 GROUP BY PERSON_ID;
```

`COALESCE(LAST_UPDATED_AT, CREATED_AT)` usa `LAST_UPDATED_AT` si existe, y recurre a `CREATED_AT` si es `NULL`. El resultado (persona + instante del último cambio) es justo lo que se convierte en cada ítem SRMD (`aboutPerson` + `someDataWasUpdatedAt` + `ofType`) que se envía a DENA.

!!! tip "Colector centralizado"

    Si tu administración tiene muchos orígenes de datos, en lugar de un componente por origen puedes desplegar un **colector de cambios centralizado**. Ver [Arquitecturas de referencia de integración](../../arquitectura/arquitecturas-referencia.md).

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
