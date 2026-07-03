# :material-translate: LanguageTexts

## Description

Key-value map for including texts in different languages. Used in service names, procedures, states, etc.

---

## JSON attributes

| Key | Type | Description |
|---|---|---|
| `SPANISH` | `String` | Text in Spanish |
| `BASQUE` | `String` | Text in Basque |
| `ENGLISH` | `String` | Text in English (optional) |

---

## Example

```json
{
    "SPANISH": "Cita previa para renovación de DNI",
    "BASQUE": "NAN berritzeko aurretiko hitzordua"
}
```

!!! tip "Minimum languages"

    It is recommended to always include at least **Spanish** (`SPANISH`) and **Basque** (`BASQUE`).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
