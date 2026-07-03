# ExportSpec

## Description

Specification of a DENA user data export request, indicating the data to export, filters to apply and the target format.

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#000000"
    primaryBorderColor: "#CCCCCC"
    lineColor: "#4A4A4A"
    fontSize: "12px"
---
flowchart LR
    EXPORT["<b>ExportSpec</b>"]

    EXPORT --> SPEC["personExportSpec"]
    EXPORT --> FORMAT["exportFileFormat"]
    EXPORT --> RANGE["lastUpdateRange<br/><i>Range</i>"]
    EXPORT --> EVENT["syncEvent"]

    style EXPORT fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style SPEC fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style FORMAT fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style RANGE fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style EVENT fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
```

| Colour | Meaning |
|--------|---------|
| 🟠 Orange | Main object (ExportSpec) |
| 🟣 Violet | Enums / states |
| 🔵 Light blue | Data fields |

---

## JSON attributes

| Field              | Type     | Mandatory | Description |
|--------------------|----------|:---------:|-------------|
| `personExportSpec` | `String` | ✅        | `data` (Includes all data for each person) <br> `sync` (Includes only creation/update timestamps) |
| `exportFileFormat` | `String` | ✅        | Format to export the data to. Possible values: `SQLITE`, `CSV`, `ZIP_OF_JSON` or `PARQUET` |
| `lastUpdateRange`  | `Range`  | ❌        | Date range to filter only persons created or updated within that range |
| `syncEvent`        | `String` | ❌        | Filter by event type. Possible values: <br> `CREATED`: New persons only <br> `DELETED`: Deleted persons only <br> `UPDATED`: Modified persons only <br> `ID_CHANGED`: Persons whose identifier (NIF, NIE, etc.) has been modified only |

## JSON example

```json
{
    "personExportSpec": "sync",
    "exportFileFormat": "CSV",
    "lastUpdateRange": "Instant:[2023-01-01T00:00:00Z..2027-01-02T00:00:00Z]",
    "syncEvent": "CREATED"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
