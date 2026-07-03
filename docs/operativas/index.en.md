# :material-cogs: DENA Operations

This section contains all the functional operations that administrations can implement to integrate with the DENA platform.

## Available Operations

<div class="grid cards" markdown>

-   :material-database: **Serve Data**
    
    ---
    
    Implement endpoints so that DENA can query data from your administration on demand.
    
    [:octicons-arrow-right-24: Go to Serve Data](../semantica/data-retrieve/index.md)

-   :material-bell: **Notify Changes**
    
    ---
    
    Notify DENA when there are changes in your administration's metadata.
    
    [:octicons-arrow-right-24: Go to Notify Changes](../semantica/metadata-sync/index.md)

-   :material-sync: **Synchronize Persons**
    
    ---
    
    Keep person information synchronized between DENA and your administration.
    
    [:octicons-arrow-right-24: Go to Synchronize Persons](../semantica/person-sync/index.md)

</div>

## Recommended Flow

1. **Start with Serve Data** - It is the fundamental operation that allows DENA to access your administration's information
2. **Implement Notify Changes** - To keep DENA informed of updates in real time  
3. **Configure Synchronize Persons** - To maintain coherence in personal data

!!! tip "Where to start?"
    If this is your first integration with DENA, we recommend starting with **Serve Data** as it is the most basic and necessary operation for any integration.
