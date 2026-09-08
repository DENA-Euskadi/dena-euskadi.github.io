# :material-cogs: Operations

This section describes the **operations your administration can implement** to integrate with DENA. Each operation is explained first conceptually and then with technical implementation detail.

---

## Recommended flow

1. **Start with Data-Retrieve** — It is the fundamental operation. Without it, DENA cannot show data from your admin.
2. **Then Metadata-Sync** — So DENA knows when there is new data without having to constantly query.
3. **Finally Person-Sync** — To know which persons are in DENA and send notifications only for them.

---

## Available operations

<div class="grid cards" markdown>

-   :material-database-arrow-right:{ .lg .middle } **Data-Retrieve (Serve Data)**

    ---

    DENA calls your admin to obtain a person's data.

    [:octicons-arrow-right-24: See detail](./data-retrieve.md)

-   :material-bell-ring:{ .lg .middle } **Metadata-Sync (Notify Changes)**

    ---

    Your admin sends DENA notifications that there is new or updated data.

    [:octicons-arrow-right-24: See detail](./metadata-sync.md)

-   :material-account-sync:{ .lg .middle } **Person-Sync (Synchronize Persons)**

    ---

    Keep the list of persons registered in DENA synchronized.

    [:octicons-arrow-right-24: See detail](./person-sync.md)

</div>

---

**Next:** [:octicons-arrow-right-24: Semantics (technical specification)](../semantica/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
