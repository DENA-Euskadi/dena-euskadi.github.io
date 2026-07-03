# :material-sync: METADATA-SYNC

> **Version:** `v{{ dena.version }}` · **Date:** {{ dena.date }}

---

## What is it?

**Metadata-Sync** is the mechanism by which administrations notify DENA when changes occur in any data associated with a user.

``` mermaid
sequenceDiagram
    participant Admin as Administration
    participant DENA as CORE DENA
    participant App as Client App

    Admin->>DENA: POST /syncMetadata (person X has changes)
    DENA-->>Admin: 200 OK

    Note over DENA: Stores metadata: person + type + date

    App->>DENA: Any updates?
    DENA-->>App: Yes, Admin X has new data for you
```

!!! info "Metadata only"

    DENA **does not store the data itself**, only the date of the last update per combination of:

    - Person
    - Data type
    - Administration

    When the client app needs the actual data, it will request it via [Data-Retrieve](../data-retrieve/index.md).

---

## Documentation

| Document | Content |
|---|---|
| [:octicons-arrow-right-24: Endpoint](./endpoint-sync-metadata.md) | REST contract for change notification |

---

!!! tip "Postman"

    Postman collection and environment available at [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
