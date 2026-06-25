# PersonHashes

## Descripción

Objeto que contiene hashes de nombre y apellidos de de una persona para su identificación inequivoca.

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

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (PersonHashes) |
| 🔵 Azul claro | Campos de datos |

---

## Atributos JSON

| Campo              | Tipo     | Obligatorio | Descripción |
|--------------------|----------|-------------|-------------|
| `nameHash`         | `String` | ✅          | Hash del nombre de la persona |
| `surname1Hash`     | `String` | ✅          | Hash del primer apellido |
| `surname2Hash`     | `String` | ❌          | Hash del segundo apellido |
| `allNamesHash`     | `String` | ✅          | Hash de la concatenación de nombre y apellidos |

## Ejemplo JSON

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
