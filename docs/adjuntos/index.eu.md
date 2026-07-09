# :material-paperclip: Eranskinak

DENAren dokumentazio teknikoaren baliabide osagarriak.

---

## :material-rocket-launch: Postman Bildumak

!!! tip "Sarbide zuzena"

    Eskuragarri hemen: [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman)

| Fitxategia | Mota | Deskribapena |
|---|---|---|
| `DENA ADMINS.postman_collection.json` | Collection | Administrazioentzako bilduma nagusia |
| `DENA-PRE-ADMINS.postman_environment.json` | Environment | PRE ingurunearen konfigurazioa |

### Funtzionalitate estaldura

- :material-check: **Data-Retrieve** — Pertsonen datuen kontsulta
- :material-check: **Metadata-Sync** — Metadatuen sinkronizazioa
- :material-check: **Person-Sync** — Pertsonen sinkronizazioa
- :material-check: **OAuth2 Autentifikazioa** — Autentifikazio fluxuak

### Nola erabili

1. Collection eta environment-a Postman-en inportatu
2. Ingurune-aldagaiak konfiguratu (URLak, kredentzialak)
3. Eskaerak exekutatu integrazioa baliozkotu

---

## :material-image-multiple: Irudiak eta Diagramak

### Arkitektura Diagramak

| Diagrama | Editagarria (.drawio) | Irudia (.png) |
|---|---|---|
| Arkitektura orokorra | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/DENA_Architecture.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/DENA_Architecture.png) |
| Login Giltza | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/login-giltza.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/login-giltza.png) |
| WebAuthn Login | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/webauthn-login.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/webauthn-login.png) |
| WebAuthn Register | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/webauthn-register.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/webauthn-register.png) |
| Person-Sync Pull | [:material-pencil:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/drawio/person-sync-pull.drawio) | [:material-image:]({{ repos.docs_blob }}/docs/adjuntos/imagenes/person-sync-pull.png) |

---

## :material-folder-outline: Egitura

```
adjuntos/
├── postman/
│   ├── DENA ADMINS.postman_collection.json
│   └── DENA-PRE-ADMINS.postman_environment.json
├── imagenes/
│   ├── drawio/            # Fitxategi editagarriak
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

## :material-open-in-new: Sarbide publikoa

Baliabide guztiak hemen daude eskuragarri:

:material-github: [DENA-Euskadi/dena-common-docs/docs/adjuntos]({{ repos.docs_tree }}/docs/adjuntos)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
