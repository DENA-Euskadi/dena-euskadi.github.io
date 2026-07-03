# :material-cube-outline: Architecture

## Overview

DENA Interop is the system that enables interoperability with different administrations to allow citizens, through the client application, to access the data that administrations manage about them.

![Architecture diagram](../adjuntos/imagenes/DENA_Architecture.png)

---

## Main modules

<div class="grid cards" markdown>

-   :material-sync:{ .lg .middle } **Metadata Sync**

    ---

    Receives notifications from administrations when there are changes in a person's data. DENA stores only the last update date per combination of person + data type + administration.

    [:octicons-arrow-right-24: Metadata-Sync Semantics](../semantica/metadata-sync/index.md)

-   :material-database-arrow-right:{ .lg .middle } **Data Retrieve**

    ---

    When the client app detects changes, it requests the administration (through DENA) to download the updated data. The administration exposes a standard endpoint.

    [:octicons-arrow-right-24: Data-Retrieve Semantics](../semantica/data-retrieve/index.md)

-   :material-account-sync:{ .lg .middle } **Person Sync**

    ---

    Synchronizes the list of registered persons in DENA with the administrations. Two mechanisms: **Pull** (the administration queries) and **Push** (DENA notifies).

    [:octicons-arrow-right-24: Person-Sync Semantics](../semantica/person-sync/index.md)

</div>

---

## General flow

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "13px"
    fontFamily: "Manrope, sans-serif"
    actorBkg: "#f5d836"
    actorBorder: "#1a4d1f"
    actorTextColor: "#1a4d1f"
    activationBkgColor: "#70d680"
    activationBorderColor: "#1a4d1f"
    sequenceNumberColor: "#1a4d1f"
---
sequenceDiagram
    participant App as Client App
    participant DENA as DENA CORE
    participant Admin as Administration

    Note over Admin,DENA: 1. Metadata Sync
    Admin->>DENA: POST /syncMetadata (there are changes)
    DENA-->>Admin: 200 OK

    Note over App,DENA: 2. App detects changes
    App->>DENA: Query metadata
    DENA-->>App: There is new data in Admin X

    Note over DENA,Admin: 3. Data Retrieve
    DENA->>Admin: POST /retrieveData (person + type)
    Admin-->>DENA: 200 OK + data
    DENA-->>App: Updated data
```

---

## Standard vs. non-standard connector

!!! tip "Standard endpoint"

    If your administration can implement the standard REST endpoint (`POST /api/retrieveData`), the integration is direct and fast.

    [:octicons-arrow-right-24: Implementation guide](../semantica/data-retrieve/guia-implementacion.md)

!!! info "Custom connector"

    If implementing the standard is not possible, DENA will develop a specific connector adapted to the means provided by the administration (SOAP, files, etc.).

---

## Related documentation

- [Diagrams (draw.io)](./diagramas.md)
- [Other documentation](./otra-documentacion.md)
- [ADRs (Architecture Decision Records)]({{ repos.docs_main }}/ArchitectureDecisionRecords)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
