# :material-paperclip: Adjuntos

Recursos complementarios a la documentación técnica de DENA.

---

## :material-rocket-launch: Colecciones Postman

!!! tip "Acceso directo"

    Disponibles en [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman)

| Archivo | Tipo | Descripción |
|---|---|---|
| `DENA ADMINS.postman_collection.json` | Collection | Colección principal para administraciones |
| `DENA-PRE-ADMINS.postman_environment.json` | Environment | Configuración de entorno PRE |

### Funcionalidades cubiertas

- :material-check: **Data-Retrieve** — Consulta de datos de personas
- :material-check: **Metadata-Sync** — Sincronización de metadatos
- :material-check: **Person-Sync** — Sincronización de personas
- :material-check: **Autenticación OAuth2** — Flujos de autenticación

### Cómo usar

1. Importar collection y environment en Postman
2. Configurar variables de entorno (URLs, credenciales)
3. Ejecutar requests para validar integración

---

## :material-image-multiple: Imágenes y Diagramas

### Diagramas de Arquitectura

| Diagrama | Editable (.drawio) | Imagen (.png) |
|---|---|---|
| Arquitectura general | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/DENA_Architecture.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/DENA_Architecture.png) |
| Login Giltza | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/login-giltza.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/login-giltza.png) |
| WebAuthn Login | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/webauthn-login.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/webauthn-login.png) |
| WebAuthn Register | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/webauthn-register.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/webauthn-register.png) |
| Person-Sync Pull | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/person-sync-pull.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/person-sync-pull.png) |

---

## :material-folder-outline: Estructura

```
adjuntos/
├── postman/
│   ├── DENA ADMINS.postman_collection.json
│   └── DENA-PRE-ADMINS.postman_environment.json
├── imagenes/
│   ├── drawio/            # Archivos editables
│   │   ├── DENA_Architecture.drawio
│   │   ├── login-giltza.drawio
│   │   ├── webauthn-login.drawio
│   │   ├── webauthn-register.drawio
│   │   └── person-sync-pull.drawio
│   ├── DENA_Architecture.png
│   ├── login-giltza.png
│   ├── webauthn-login.png
│   ├── webauthn-register.png
│   └── person-sync-pull.png
└── index.md
```

---

## :material-open-in-new: Acceso público

Todos los recursos están disponibles en:

:material-github: [DENA-Euskadi/dena-common-docs/tree/{{ tags.dena_common_docs }}/docs/adjuntos]({{ repos.docs_tree }}/docs/adjuntos)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
