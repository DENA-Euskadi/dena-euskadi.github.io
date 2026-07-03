# :material-upload: PERSON-SYNC — PUSH

DENA proactively notifies the administration when a new person registers or changes occur in their data.

---

## Flow

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA
    participant Admin as Administration

    Note over DENA: New person registers / change occurs
    DENA->>Admin: POST /api/person-push (change details)
    Admin-->>DENA: 200 OK
```

---

## What does the administration need?

!!! info "Implement an endpoint"

    The administration must expose a REST endpoint capable of receiving the change details.
    DENA will invoke this endpoint every time a person registration or modification occurs.

---

## Endpoint contract

[:octicons-arrow-right-24: Endpoint Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) — Request, response, JSON examples and HTTP codes.

---

!!! tip "When to use Push"

    - When you need to react **in real time** to person changes
    - When you don't want to depend on periodic files (Pull)
    - When your system needs the data immediately to notify changes via Metadata-Sync

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
