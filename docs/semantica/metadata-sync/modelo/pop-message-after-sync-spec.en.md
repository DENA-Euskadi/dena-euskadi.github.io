# PopMessageAfterSyncSpec

## Description

Representation of a change on a piece of data managed by an administration associated with a DENA user.

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
    MSG["<b>PopMessageAfterSyncSpec</b>"]

    MSG --> HOW["how"]
    MSG --> LANG["messageByLang<br/><i>LanguageTexts</i>"]

    style MSG fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style HOW fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style LANG fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click LANG "../../../semantica-base/modelo/language-texts/" "View LanguageTexts"
```

| Colour | Meaning |
|-------|-------------|
| 🟠 Orange | Main object (PopMessageAfterSyncSpec) |
| 🟣 Purple | Enums / defined values |
| 🔵 Light blue | Data fields |

---

## JSON attributes

| Field    | Type     | Mandatory | Description |
|----------|----------|-------------|-------------|
| `how`    | `String` | ✅          | Indication of how the message will be displayed to the user. Possible values: <br> `PUSH_TO_CLIENT_AT_CORE`: The message will be notified via PUSH to the client <br> `AT_CLIENT_AFTER_SYNC`: The message will be displayed after performing a synchronization from the client |
| `messageByLang` | [LanguageTexts](../../semantica-base/modelo/language-texts.md) | ✅ | Key-value map with the message in the different possible languages |

---

## JSON example

```json
{
    "how": "AT_CLIENT_AFTER_SYNC",
    "messageByLang": {
        "SPANISH": "Nuevo expediente de \"adminId\"",
        "ENGLISH": "New procedure from \"adminId\""
    }
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
