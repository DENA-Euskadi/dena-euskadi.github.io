# PersonHashes

## Deskribapena

Pertsona baten izen eta abizenen hash-ak dituen objektua, modu zalantzarik gabean identifikatzeko.

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

| Kolorea | Esanahia |
|-------|-------------|
| 🟠 Laranja | Objektu nagusia (PersonHashes) |
| 🔵 Urdin argia | Datu-eremuak |

---

## JSON atributuak

| Eremua             | Mota     | Nahitaezkoa | Deskribapena |
|--------------------|----------|-------------|-------------|
| `nameHash`         | `String` | ✅          | Pertsonaren izenaren hash-a |
| `surname1Hash`     | `String` | ✅          | Lehen abizenaren hash-a |
| `surname2Hash`     | `String` | ❌          | Bigarren abizenaren hash-a |
| `allNamesHash`     | `String` | ✅          | Izen eta abizenen kateaketaren hash-a |

## JSON adibidea

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
