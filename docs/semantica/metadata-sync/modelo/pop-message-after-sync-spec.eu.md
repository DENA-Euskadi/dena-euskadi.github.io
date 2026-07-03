# PopMessageAfterSyncSpec

## Deskribapena

DENA erabiltzaile bati lotutako administrazio batek kudeatutako datu baten aldaketa baten adierazpena.

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

    click LANG "../../../semantica-base/modelo/language-texts/" "Ikusi LanguageTexts"
```

| Kolorea | Esanahia |
|-------|-------------|
| 🟠 Laranja | Objektu nagusia (PopMessageAfterSyncSpec) |
| 🟣 Morea | Enum-ak / definitutako balioak |
| 🔵 Urdin argia | Datu-eremuak |

---

## JSON atributuak

| Eremua    | Mota     | Nahitaezkoa | Deskribapena |
|----------|----------|-------------|-------------|
| `how`    | `String` | ✅          | Mezua erabiltzaileari nola erakutsiko zaion adierazten du. Balio posibleak: <br> `PUSH_TO_CLIENT_AT_CORE`: Mezua PUSH bidez jakinaraziko zaio bezeroari <br> `AT_CLIENT_AFTER_SYNC`: Mezua bezerotik sinkronizazio bat egin ondoren erakutsiko da |
| `messageByLang` | [LanguageTexts](../../semantica-base/modelo/language-texts.md) | ✅ | Gako-balio mapa mezuarekin hizkuntza posible ezberdinetan |

---

## JSON adibidea

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
