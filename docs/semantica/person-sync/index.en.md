# :material-account-sync: PERSON-SYNC

> **Version:** `v{{ dena.version }}` · **Date:** {{ dena.date }}

---

## What is it?

**Person-Sync** allows administrations to synchronize the persons registered in DENA into their systems, so they can notify DENA of updates to the data associated with those persons.

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
    DENA[CORE DENA] -->|Push: notifies changes| Admin[Administration]
    Admin -->|Pull: downloads persons| DENA
    
    style DENA fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style Admin fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## Mechanisms

=== ":material-download: Pull (Administration → DENA)"

    The administration connects to DENA and downloads person data.

    - Pre-generated files with incremental changes (periodic)
    - Possibility to request custom files with personalized filters

    [:octicons-arrow-right-24: Pull documentation](./pull.md)

=== ":material-upload: Push (DENA → Administration)"

    DENA proactively notifies the administration when a new person registers or changes occur.

    - The administration exposes a reception endpoint
    - DENA sends the notification at the time of the change

    [:octicons-arrow-right-24: Push documentation](./push.md)

---

## Endpoints

### Pull

| Document | Content |
|---|---|
| [Fetch Persons Pregen Export Asset](./endpoints/pull/fetch-persons-pregen-export-asset.md) | Download of pre-generated files |
| [Create Pull from Admin Bespoke Job](./endpoints/pull/create-pull-from-admin-bespoke-job.md) | Custom export request |
| [Get Pull from Admin Bespoke Job](./endpoints/pull/get-pull-from-admin-bespoke-job.md) | Request status query |
| [Fetch Persons Bespoke Export Asset](./endpoints/pull/fetch-persons-bespoke-export-asset.md) | Download of custom files |

### Push

| Document | Content |
|---|---|
| [Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) | Reception endpoint contract |

---

!!! tip "Postman"

    Postman collection and environment available at [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
