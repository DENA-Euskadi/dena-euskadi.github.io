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

    Admin->>DENA: POST /api/admin/interop/sync/metadata (person X has changes)
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

## How to detect changes in your administration

The first thing a data origin has to do to integrate is to **detect what has changed**. For DENA, the only information needed is:

- **Which person** has some modified data (per data type).
- **When** the last change happened.

The **specific data** that changed **does not matter**: only the fact that a change occurred in the data origin and when. An inserted or deleted row also counts as a change.

This is usually a simple SQL query per data type. For example, for a business table with this structure:

| PERSON_ID | PERSON_DATA | CREATED_AT | LAST_UPDATED_AT |
|-----------|-------------|------------|-----------------|
| 48291038Z | … | 2026-08-17T03:22:10Z | 2026-08-17T09:14:22Z |
| 10593847H | … | 2026-08-17T14:05:49Z | 2026-08-17T18:41:03Z |

the query that returns the people with changes after a given date is straightforward:

```sql
SELECT PERSON_ID,
       MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE_AT
  FROM DB_TABLE
 WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= :from
 GROUP BY PERSON_ID;
```

`COALESCE(LAST_UPDATED_AT, CREATED_AT)` uses `LAST_UPDATED_AT` if present, falling back to `CREATED_AT` if it is `NULL`. The result (person + instant of the last change) is exactly what becomes each SRMD item (`aboutPerson` + `someDataWasUpdatedAt` + `ofType`) sent to DENA.

!!! tip "Centralized collector"

    If your administration has many data origins, instead of one component per origin you can deploy a **centralized changes collector**. See [Integration reference architectures](../../arquitectura/arquitecturas-referencia.md).

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
