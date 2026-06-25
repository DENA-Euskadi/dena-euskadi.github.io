# :material-account-sync: PERSON-SYNC

> **Versión:** `v{{ dena.version }}` · **Fecha:** {{ dena.date }}

---

## ¿Qué es?

**Person-Sync** permite a las administraciones sincronizar las personas registradas en DENA en sus sistemas, para poder notificar a DENA las actualizaciones en los datos asociados a esas personas.

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "14px"
    fontFamily: "Manrope, sans-serif"
---
graph LR
    DENA[CORE DENA] -->|Push: notifica cambios| Admin[Administración]
    Admin -->|Pull: descarga personas| DENA
    
    style DENA fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style Admin fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## Mecanismos

=== ":material-download: Pull (Administración → DENA)"

    La administración se conecta a DENA y descarga los datos de personas.

    - Ficheros pregenerados con cambios incrementales (periódicos)
    - Posibilidad de solicitar ficheros a medida con filtros personalizados

    [:octicons-arrow-right-24: Documentación Pull](./pull.md)

=== ":material-upload: Push (DENA → Administración)"

    DENA notifica proactivamente a la administración cuando se registra una persona nueva o se producen cambios.

    - La administración expone un endpoint de recepción
    - DENA envía la notificación en el momento del cambio

    [:octicons-arrow-right-24: Documentación Push](./push.md)

---

## Endpoints

### Pull

| Documento | Contenido |
|---|---|
| [Fetch Persons Pregen Export Asset](./endpoints/pull/fetch-persons-pregen-export-asset.md) | Descarga de ficheros pregenerados |
| [Create Pull from Admin Bespoke Job](./endpoints/pull/create-pull-from-admin-bespoke-job.md) | Solicitud de exportación a medida |
| [Get Pull from Admin Bespoke Job](./endpoints/pull/get-pull-from-admin-bespoke-job.md) | Consulta de estado de solicitudes |
| [Fetch Persons Bespoke Export Asset](./endpoints/pull/fetch-persons-bespoke-export-asset.md) | Descarga de ficheros a medida |

### Push

| Documento | Contenido |
|---|---|
| [Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) | Contrato del endpoint de recepción |

---

!!! tip "Postman"

    Colección y environment Postman disponibles en [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
