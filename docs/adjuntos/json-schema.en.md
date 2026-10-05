# :material-code-json: REST messages JSON Schema

**JSON Schema (draft 2020-12)** describing the structure of every DENA interoperability REST message and of the data models they exchange (records, notices, payments, people, appointments, etc.).

!!! info "What is it for?"

    This schema is a reference **generated from the real DENA CORE models**. You can use it to:

    - **Validate** the messages your administration sends or receives (request/response) before integrating them.
    - **Generate types / clients** in your language (TypeScript, Java, Python…) from the schema.
    - **Look up** at a glance the fields, types and allowed enum values.

---

## Download

| Resource | Description | Download |
|----------|-------------|:--------:|
| **DENA REST Messages — JSON Schema** | Schema of all REST messages (data-retrieve, metadata-sync, person-sync, person fetch/head/search) and of the exchanged data models. | [:material-download: JSON Schema](./json-schema/dena-rest-messages.schema.json) |

---

## Included messages

The schema describes, among others, these top-level messages:

| Operation | Message (request / response) |
|-----------|------------------------------|
| **Data-Retrieve** | `DN00COREToConnectorDataRetrieveRequestMessage` / `DN00COREToConnectorDataRetrieveResponseMessage` |
| **Metadata-Sync (SRMD)** | `DN00SyncMetaDataFromAdminRequestMessage` / `DN00SyncMetaDataFromAdminResponseMessage` |
| **Person-Sync (push)** | `DN00PersonInteropPushToAdminNotificationMessage` |
| **Person-Sync (pull bespoke)** | `DN00PersonInteropPullFromAdminBespokeJobCreate…` / `…BespokeJobGet…` / `…BespokeExportAssetFetch…` |
| **Person-Sync (pull pregen)** | `DN00PersonInteropPullFromAdminPreGenJobGet…` / `…PreGenExportAssetFetch…` |
| **Person (fetch / head / search)** | `DN00PersonFetchInterop…` / `DN00PersonHeadInterop…` / `DN00PersonInteropSearch…` |

Each exchanged data object carries a **type discriminator** (field `type`), e.g. `administrativeServiceProcedureRecord`, `administrativeNotice`, `payment`, `directDebitPayment`, `personData` or `scheduleItem`.

---

!!! tip "Detailed semantics"

    The functional, field-by-field description of each message and model lives in the [Semantics](../semantica/index.md) section. This schema is the **formal, validatable** complement to that documentation.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
