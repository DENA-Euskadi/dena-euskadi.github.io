# :material-sync: Sync Flow Data Types

This page documents the data structures used in the **metadata synchronization flow** (Metadata-Sync / SRMD) between administrations and DENA-CORE.

---

## DenaInteroperableDataTypeSync

Structure used by a data source / administration to send **meta-data sync for an interoperable data type**, reporting changes, additions or deletions in data for persons registered in DENA.

### Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `interoperableDataTypeId` | `ID` | :material-check: | Data type identifier (e.g. `PAYMENTS`, `RECORDS`) |
| `personDataSync` | `Array(DenaPersonDataSync)` | :material-check: | Array with detected changes per person |

### Example

```json
{
  "interoperableDataTypeId": "RECORDS",
  "personDataSync": [
    {
      "personId": "11111111H",
      "lastUpdated": "2025-07-16T13:00:00.000Z",
      "change": "CHANGED"
    },
    {
      "personId": "11223344L",
      "lastUpdated": "2025-07-15T11:00:00.000Z",
      "change": "NEW"
    }
  ]
}
```

---

## DenaPersonDataSync

Structure reporting changes in data for **one person** for an interoperable data type.

### Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `personId` | `ID` | :material-check: | Administrative identifier of the person (DNI/NIF/NIE/Passport) |
| `objectOid` | `OID` | :material-close: | Unique object identifier in DENA's person module |
| `lastUpdated` | `UTCDateTime` | :material-check: | Date/time of the last data update |
| `change` | `Enum` | :material-check: | Type of change detected |

### `change` values

| Value | Description |
|---|---|
| `CHANGED` | An existing record has been modified |
| `NEW` | A new record has been created |
| `DELETED` | A record has been deleted |

---

## DenaPersonMetaDataSyncItem

Structure used by the UI (DENA-APP) to request meta-data sync **for a person** across all interoperable data types from all data sources.

### Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `adminId` | `ID` | :material-check: | Administration identifier in DENA |
| `dataSourceId` | `ID` | :material-check: | Data source identifier in DENA |
| `dataTypeId` | `ID` | :material-check: | Data type identifier in DENA |
| `lastUpdateTS` | `TimeStamp` | :material-check: | Date of last modification of any record of the data type in the source |

### Example

```json
[
  {
    "adminId": "EJGV",
    "dataSourceId": "EJGV-Expedientes",
    "dataTypeId": "RECORDS",
    "lastUpdateTS": 1670374400
  },
  {
    "adminId": "DFB",
    "dataSourceId": "DFB-Pagos",
    "dataTypeId": "PAYMENTS",
    "lastUpdateTS": 1670380000
  }
]
```

!!! info "UI usage"
    The UI uses `lastUpdateTS` to determine if it needs to RETRIEVE data from the source. To perform the retrieve, the UI needs to know the retrieve URL in DENA-CORE for each data source (available in the data source registry).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
