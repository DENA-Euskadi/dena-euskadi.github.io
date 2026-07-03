# :material-paperclip: Attachments

Complementary resources to DENA's technical documentation.

---

## :material-rocket-launch: Postman Collections

!!! tip "Direct access"

    Available at [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman)

| File | Type | Description |
|---|---|---|
| `DENA ADMINS.postman_collection.json` | Collection | Main collection for administrations |
| `DENA-PRE-ADMINS.postman_environment.json` | Environment | PRE environment configuration |

### Covered features

- :material-check: **Data-Retrieve** — Person data query
- :material-check: **Metadata-Sync** — Metadata synchronization
- :material-check: **Person-Sync** — Person synchronization
- :material-check: **OAuth2 Authentication** — Authentication flows

### How to use

1. Import collection and environment into Postman
2. Configure environment variables (URLs, credentials)
3. Execute requests to validate integration

---

## :material-image-multiple: Images and Diagrams

### Architecture Diagrams

| Diagram | Editable (.drawio) | Image (.png) |
|---|---|---|
| General architecture | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/DENA_Architecture.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/DENA_Architecture.png) |
| Login Giltza | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/login-giltza.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/login-giltza.png) |
| WebAuthn Login | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/webauthn-login.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/webauthn-login.png) |
| WebAuthn Register | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/webauthn-register.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/webauthn-register.png) |
| Person-Sync Pull | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/person-sync-pull.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/person-sync-pull.png) |

---

## :material-folder-outline: Structure

```
adjuntos/
├── postman/
│   ├── DENA ADMINS.postman_collection.json
│   └── DENA-PRE-ADMINS.postman_environment.json
├── imagenes/
│   ├── drawio/            # Editable files
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

## :material-open-in-new: Public access

All resources are available at:

:material-github: [DENA-Euskadi/dena-common-docs/docs/adjuntos]({{ repos.docs_tree }}/docs/adjuntos)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
