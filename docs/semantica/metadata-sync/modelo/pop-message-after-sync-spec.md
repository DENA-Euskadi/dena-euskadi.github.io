# PopMessageAfterSyncSpec

## Descripción

Representación de un cambio sobre un dato gestionado por una administración asociado a una persona usuaria de DENA.

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

    click LANG "../semantica-base/modelo/language-texts.md" "Ver LanguageTexts"
```

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (PopMessageAfterSyncSpec) |
| 🟣 Violeta | Enums / valores definidos |
| 🔵 Azul claro | Campos de datos |

---

## Atributos JSON

| Campo    | Tipo     | Obligatorio | Descripción |
|----------|----------|-------------|-------------|
| `how`    | `String` | ✅          | Indicación de como se mostrará el mensaje a la persona usuaria. Valores posibles: <br> `PUSH_TO_CLIENT_AT_CORE`: El mensaje se notificara por PUSH al cliente <br> `AT_CLIENT_AFTER_SYNC`: El mensaje se mostrara tras realizar una sincronización desde el cliente |
| `messageByLang` | [LanguageTexts](../semantica-base/modelo/language-texts.md) | ✅ | Mapa clave-valor con el mensaje en los distintos idiomas posibles |

---

## Ejemplo JSON

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
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
