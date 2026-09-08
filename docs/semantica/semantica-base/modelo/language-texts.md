# :material-translate: LanguageTexts

## Descripción

Mapa clave-valor para incluir textos en diferentes idiomas. Se utiliza en nombres de servicios, procedimientos, estados, etc.

---

## Atributos JSON

La clave es un valor del enum `Language`. Los valores disponibles son:

| Clave | Tipo | Descripción |
|---|---|---|
| `SPANISH` | `String` | Texto en castellano |
| `BASQUE` | `String` | Texto en euskera |
| `ENGLISH` | `String` | Texto en inglés |
| `FRENCH` | `String` | Texto en francés |
| `DEUTCH` | `String` | Texto en alemán |

---

## Ejemplo

```json
{
    "SPANISH": "Cita previa para renovación de DNI",
    "BASQUE": "NAN berritzeko aurretiko hitzordua"
}
```

!!! tip "Idiomas mínimos"

    Se recomienda incluir siempre **castellano** (`SPANISH`) y **euskera** (`BASQUE`) como mínimo.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
