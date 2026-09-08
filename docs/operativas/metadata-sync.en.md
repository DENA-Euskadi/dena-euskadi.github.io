# :material-bell-ring: Metadata-Sync (Notify Changes)

## Concept

Metadata-Sync is the operation by which **your administration notifies DENA that there is new or updated data** for certain persons. Without this, DENA does not know when there are updates and the app cannot notify the person that there is fresh data.

![SRMD High Level](../adjuntos/imagenes/arquitectura/services-srmd-high-level.png)

### How does it work?

Your admin **periodically** (as frequently as possible):

1. Queries its DB looking for data that has changed since the last cycle
2. For each change, creates an SRMD record:
    ```
    {person} | {data type} | {admin} | {last update instant}
    ```
3. Sends all SRMDs to DENA-CORE via REST

![SRMD Strategies](../adjuntos/imagenes/arquitectura/services-srmd-strategies.png)

### Key points

- You only send **meta-data** (what changed and when), NOT the data itself nor who the person specifically is
- You can send SRMDs for **multiple data types** and **multiple persons** in a single call
- If a record is new OR has been modified, it counts as a change
- Deleted records also count as a change
- The ideal frequency is: as often as you can (every minute, every 5 minutes, every hour...)

---

## Implementation

### Step 1: Query changes in your DB

The only information DENA needs is: **what data type was modified?** and **when was the last change?**

```sql
SELECT PERSON_ID,
       MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE_AT
  FROM MY_TABLE
 WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= ?
 GROUP BY PERSON_ID;
```

!!! note ""
    `COALESCE(LAST_UPDATED_AT, CREATED_AT)` uses the update date if it exists, or the creation date if it has never been updated.

### Step 2: Convert to DENA model

```java
private List<DN00SyncMetaDataFromAdminToCOREItem> _toSRMD(
        final Collection<DBPersonLastChange> dbChanges) {
    return dbChanges.stream()
        .map(dbChange -> DN00SyncMetaDataBuilder
            .syncMetaDataFromAdminToCOREBuilder()
            .atAdmin(DN00OrgAdminID.forId("MY_ADMIN"))
            .fromSingleDataOrigin()
            .aboutPerson(DN00PersonID.forId(dbChange.personId()))
            .someDataWasUpdatedAt(dbChange.lastChangedAt())
            .ofType(DN00DataTypeID.forId("citizen_service_appointment"))
            .build())
        .toList();
}
```

### Step 3: Wrap in an interop message and send

```java
// Create the interop message
DN00InteropContext ctx = DN00InteropContextBuilder
    .createMessageOfType(DN00InteropMessageType.ADMIN_SYNC_METADATA_REQ)
    .withCorrelationOID(DN00MessageCorrelationOID.supply())
    .fromAdministration(DN00OrgAdminID.forId("MY_ADMIN"))
    .build();

DN00SyncMetaDataFromAdminRequestPayload payload =
    DN00SyncMetaDataFromAdminRequestPayload.from(srmdItems);

DN00SyncMetaDataFromAdminRequestMessage reqMsg =
    new DN00SyncMetaDataFromAdminRequestMessage(ctx, payload);

// Serialize to JSON
String json = marshaller.forWriting().toJSON(reqMsg);

// Send via HTTP POST
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.dena.eus/srmd/"))
    .header("Content-Type", "application/json")
    .header("Accept", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(json))
    .build();
```

### Example: JSON sent to DENA

```json
[
  {
    "admin": { "id": "EJGV" },
    "aboutPerson": { "id": "40404040H" },
    "someDataWasUpdatedAt": "2026-08-17T15:14:07.036Z",
    "ofType": { "id": "ADMIN_NOTICE" },
    "fromDataOrigin": "DEFAULT"
  },
  {
    "admin": { "id": "EJGV" },
    "aboutPerson": { "id": "12345678A" },
    "someDataWasUpdatedAt": "2026-08-17T15:14:07.032Z",
    "ofType": { "id": "PROCEDURE_RECORD" },
    "fromDataOrigin": "DEFAULT"
  }
]
```

!!! tip "IDs vs OIDs"
    - The **person id** is their NIF (e.g.: `12345678A`)
    - The **administration id** is always its NIF/CIF (e.g.: `S4833001C` for EJGV)
    - The **data type id** is the identifier agreed upon with the DENA team (e.g.: `PROCEDURE_RECORD`)
    - The **fromDataOrigin** is `DEFAULT` if you only have one data origin. If you have multiple origins, use the identifier previously communicated to the DENA team
    
    You do not need to use DENA's internal OIDs. Business IDs are sufficient.

!!! note "About fromDataOrigin"
    If your administration only has one data origin per data type (the most common case), send `"fromDataOrigin": "DEFAULT"` and you don't need to worry about anything else.
    
    If you have **multiple origins** for the same data type (e.g.: several case management systems), the `fromDataOrigin` field must contain the identifier of the specific origin. This identifier is agreed upon with the DENA team during connector configuration. If you do not communicate it beforehand, the items will fail during processing.

### Example: DENA response

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 2,
  "processedOK": [ ... ],
  "processedNOK": [
    {
      "item": { ... },
      "error": "The [data type] with ref=unknown could NOT be validated"
    }
  ]
}
```

DENA returns which items were processed correctly (`processedOK`) and which ones failed (`processedNOK`) with the error reason. A submission can have both valid and invalid items simultaneously: valid ones are processed normally and invalid ones are rejected individually without affecting the rest.

!!! warning "Classic error: data origin not configured"
    The most common failure in `processedNOK` is a `fromDataOrigin` that is not registered in the DENA configuration. If you receive validation errors on the data origin, verify with the DENA team that the identifier you are using matches the one configured in the connector.

---

## Full specification

For the detailed specification of the endpoint, model and errors:

[:octicons-arrow-right-24: Metadata-Sync Semantics](../semantica/metadata-sync/index.md)

---

**Next:** [:octicons-arrow-right-24: Person-Sync](./person-sync.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
