# PersonHashes

## Description

Object containing name and surname hashes of a person for their unambiguous identification.

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
    HASHES["<b>PersonHashes</b>"]

    HASHES --> NAME["nameHash"]
    HASHES --> SURNAME1["surname1Hash"]
    HASHES --> SURNAME2["surname2Hash"]
    HASHES --> ALLNAMES["allNamesHash"]

    style HASHES fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style NAME fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style SURNAME1 fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style SURNAME2 fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style ALLNAMES fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
```

| Colour | Meaning |
|-------|-------------|
| 🟠 Orange | Main object (PersonHashes) |
| 🔵 Light blue | Data fields |

---

## JSON attributes

| Field              | Type     | Mandatory | Description |
|--------------------|----------|-------------|-------------|
| `nameHash`         | `String` | ✅          | Hash of the person's name |
| `surname1Hash`     | `String` | ✅          | Hash of the first surname |
| `surname2Hash`     | `String` | ❌          | Hash of the second surname |
| `allNamesHash`     | `String` | ✅          | Hash of the concatenation of name and surnames |

## JSON example

```json
{
    "nameHash": "abcde",
    "surname1Hash": "abcde",
    "surname2Hash": "abcde",
    "allNamesHash": "abcde"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
