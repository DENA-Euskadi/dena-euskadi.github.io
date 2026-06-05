# Cita (Schedule Item)

## Descripción

Representa un elemento de agenda o cita previa. Puede ser una cita con duración o un hito puntual (duración = 0).

> **Nota:** A diferencia de los demás objetos (notificación, registro, pago), las citas son **independientes de expedientes**. No tienen campo `procedureRecord`.

> Ver también: [campos-comunes.md](./campos-comunes.md) para campos heredados (`oid`, `id`, `urls`, `originAdminRef`, `aboutPersonRef`)

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
    CITA["<b>Cita</b><br/><i>scheduleItem</i>"]

    CITA --> WHEN["year · monthOfYear<br/>dayOfMonth · hourOfDay<br/>minuteOfHour · durationMinutes"]
    CITA --> PRIO["priority"]
    CITA --> SUBJECT["subject<br/>details"]
    CITA --> LOC["location<br/><i>country · administrativeAreaLevel1<br/>administrativeAreaLevel3 · zipCode<br/>address · directionsByLanguage</i>"]
    CITA --> URLS["urls[]"]

    style CITA fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style WHEN fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style PRIO fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
    style SUBJECT fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style LOC fill:#d5e8d4,stroke:#82b366,color:#000000,rx:6,ry:6
    style URLS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
```

| Color | Significado |
|-------|-------------|
| 🟠 Naranja | Objeto principal (Cita) |
| 🟣 Violeta | Enums (prioridad) |
| 🔵 Azul claro | Campos de datos |
| 🟢 Verde | Ubicación física |

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `type` | `String` | ✅ | `"scheduleItem"` |
| `oid` | `String` | ✅ | Identificador técnico único |
| `id` | `String` | ✅ | Identificador de negocio |
| `year` | `Number` | ✅ | Año |
| `monthOfYear` | `Number` | ✅ | Mes (1-12) |
| `dayOfMonth` | `Number` | ✅ | Día del mes (1-31) |
| `hourOfDay` | `Number` | ✅ | Hora (0-23) |
| `minuteOfHour` | `Number` | ✅ | Minuto (0-59) |
| `durationMinutes` | `Number` | ✅ | Duración en minutos (0 = hito puntual) |
| `priority` | `String` | ❌ | Prioridad |
| `subject` | `LanguageTexts` | ✅ | Asunto de la cita |
| `details` | `LanguageTexts` | ❌ | Detalles adicionales |
| `location` | `Object` | ❌ | Ubicación (recomendado) |
| `urls` | `Array` | ❌ | URLs de acceso |

---

## Prioridad (`priority`)

| Código | Descripción |
|--------|-------------|
| `HIGH` | Alta |
| `MEDIUM` | Media |
| `NORMAL` | Normal |
| `LOW` | Baja |

---

## Ubicación (`location`)

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `country` | `Object` | ❌ | País (`id`, `name`) |
| `administrativeAreaLevel1` | `Object` | ❌ | Territorio / CCAA (`id`, `name`) |
| `administrativeAreaLevel3` | `Object` | ❌ | Municipio (`id`, `name`) |
| `zipCode` | `String` | ❌ | Código postal |
| `address` | `String` | ❌ | Dirección |
| `directionsByLanguage` | `LanguageTexts` | ❌ | Indicaciones de acceso multiidioma (serializado como `"address"` en JSON — ver nota) |

> **Nota:** En el modelo actual, tanto `address` (String) como `directionsByLanguage` (LanguageTexts) se serializan con el nombre JSON `"address"` debido a un bug conocido en el código. Se recomienda usar solo uno de los dos campos hasta que se corrija.

---

## Ejemplo JSON

```json
{
  "type": "scheduleItem",
  "oid": "SCHED-OID-001",
  "id": "CITA-2024-00050",
  "year": 2024,
  "monthOfYear": 7,
  "dayOfMonth": 15,
  "hourOfDay": 10,
  "minuteOfHour": 30,
  "durationMinutes": 30,
  "priority": "NORMAL",
  "subject": {
    "SPANISH": "Cita previa para renovación de DNI",
    "BASQUE": "NAN berritzeko aurretiko hitzordua"
  },
  "details": {
    "SPANISH": "Traer fotografía reciente y DNI anterior",
    "BASQUE": "Ekarri argazki berria eta aurreko NANa"
  },
  "location": {
    "country": { "id": "ES", "name": "España" },
    "administrativeAreaLevel1": { "id": "PV", "name": "País Vasco" },
    "administrativeAreaLevel3": { "id": "48020", "name": "Bilbao" },
    "zipCode": "48001",
    "address": "Gran Vía 50, Bilbao",
    "directionsByLanguage": {
      "SPANISH": "Planta 2, oficina 201. Acceso por la entrada principal.",
      "BASQUE": "2. solairua, 201 bulegoa. Sarrera nagusitik sartu."
    }
  },
  "urls": [
    { "url": "https://sede.miadmin.eus/cita/CITA-2024-00050", "language": "SPANISH", "tags": ["default"] }
  ]
}
```

---

## Ejemplo — Hito (duración = 0)

```json
{
  "type": "scheduleItem",
  "oid": "SCHED-OID-002",
  "id": "HITO-2024-00010",
  "year": 2024,
  "monthOfYear": 9,
  "dayOfMonth": 1,
  "hourOfDay": 0,
  "minuteOfHour": 0,
  "durationMinutes": 0,
  "subject": {
    "SPANISH": "Inicio del plazo de matriculación",
    "BASQUE": "Matrikulazio-epearen hasiera"
  }
}
```

---

## Diferencias con otros objetos

| Característica | Cita | Expediente / Notificación / Registro / Pago |
|----------------|------|----------------------------------------------|
| Depende de expediente | ❌ No | ✅ Sí (campo `procedureRecord`) |
| Tiene estado | ❌ No | ✅ Sí |
| Tiene fecha ISO 8601 | ❌ (usa campos separados year/month/day/hour/minute) | ✅ Sí |
| Tiene ubicación física | ✅ Sí | ❌ No |
