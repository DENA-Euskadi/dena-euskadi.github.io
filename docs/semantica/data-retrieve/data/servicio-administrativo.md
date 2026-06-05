# Servicio Administrativo (Administrative Service)

## Descripción

El **Servicio** y el **Procedimiento** forman el contexto jerárquico del expediente. Todo expediente pertenece a un procedimiento, y todo procedimiento pertenece a un servicio.

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
flowchart TD
    SRV["<b>Servicio</b><br/><i>serviceNameByLanguage<br/>originRef · DENARef · SIARef</i>"]
    PROC["<b>Procedimiento</b><br/><i>serviceNameByLanguage<br/>originRef · DENARef · SIARef</i>"]
    EXP["<b>Expediente</b> (1:N)"]

    SRV --> PROC
    PROC --> EXP

    EXP -.-> NOT["Notificaciones"]
    EXP -.-> REG["Registros oficiales"]
    EXP -.-> PAY["Pagos"]

    SRV --> ORG["participatingOrgUnits[]<br/><i>orgUnit · role</i>"]
    SRV --> REFS["originRef / DENARef / SIARef<br/><i>oid · id · urls</i>"]

    style SRV fill:#f5f5f5,stroke:#666666,color:#000000,rx:8,ry:8
    style PROC fill:#f5f5f5,stroke:#666666,color:#000000,rx:8,ry:8
    style EXP fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style NOT fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style REG fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style PAY fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style ORG fill:#d5e8d4,stroke:#82b366,color:#000000,rx:6,ry:6
    style REFS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click EXP "./expediente.md" "Ver Expediente"
    click NOT "./notificacion.md" "Ver Notificación"
    click REG "./registro-oficial.md" "Ver Registro Oficial"
    click PAY "./pago.md" "Ver Pago"
    click ORG "./unidad-organica.md" "Ver Unidad Orgánica"
```

| Color | Significado |
|-------|-------------|
| ⚪ Gris | Contexto jerárquico (Servicio / Procedimiento) |
| 🟠 Naranja | Objeto de dominio (Expediente) |
| 🟡 Amarillo | Objetos dependientes (Notificación, Registro, Pago) |
| 🟢 Verde | Unidades orgánicas |
| 🔵 Azul claro | Referencias (originRef / DENARef / SIARef) |

---

## Servicio — Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `serviceNameByLanguage` | `LanguageTexts` | ✅ | Nombre del servicio |
| `serviceUrls` | `Array` | ❌ | URLs del catálogo de servicios |
| `participatingOrgUnits` | `Array` | ❌ | Unidades orgánicas participantes (ver [unidad-organica.md](./unidad-organica.md)) |
| `originRef` | `Object` | ✅ | Referencia en la administración de origen |
| `originRef.oid` | `String` | ❌ | OID en el catálogo de origen |
| `originRef.id` | `String` | ✅ | ID en el catálogo de origen |
| `originRef.urls` | `Array` | ❌ | URLs en el catálogo de origen |
| `DENARef` | `Object` | ❌ | Referencia en el catálogo DENA (si existe) |
| `DENARef.oid` | `String` | ❌ | OID en el catálogo DENA |
| `DENARef.id` | `String` | ❌ | ID en el catálogo DENA |
| `SIARef` | `Object` | ❌ | Referencia en el catálogo SIA de la AGE (si existe) |
| `SIARef.oid` | `String` | ❌ | OID en el catálogo SIA |
| `SIARef.id` | `String` | ❌ | ID en el catálogo SIA |

---

## Procedimiento — Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|-------|------|:-----------:|-------------|
| `serviceNameByLanguage` | `LanguageTexts` | ✅ | Nombre del procedimiento |
| `serviceUrls` | `Array` | ❌ | URLs del catálogo |
| `participatingOrgUnits` | `Array` | ❌ | Unidades orgánicas participantes |
| `originRef` | `Object` | ✅ | Referencia en la administración de origen |
| `originRef.oid` | `String` | ❌ | OID en el catálogo de origen |
| `originRef.id` | `String` | ✅ | ID en el catálogo de origen |
| `originRef.urls` | `Array` | ❌ | URLs en el catálogo de origen |
| `DENARef` | `Object` | ❌ | Referencia en el catálogo DENA (si existe) |
| `SIARef` | `Object` | ❌ | Referencia en el catálogo SIA (si existe) |

---

## Estructura de las referencias (`originRef`, `DENARef`, `SIARef`)

Todas las referencias comparten la misma estructura:

```json
{
  "oid": "SRV-OID-001",
  "id": "SRV-LIC-ACT",
  "urls": [
    { "url": "https://catalogo.miadmin.eus/servicio/SRV-LIC-ACT", "language": "SPANISH" }
  ]
}
```

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `oid` | `String` | Identificador técnico en el catálogo |
| `id` | `String` | Identificador de negocio en el catálogo |
| `urls` | `Array` | URLs de acceso al catálogo |

---

## Ejemplo JSON (dentro de un expediente)

```json
{
  "service": {
    "serviceNameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
    "originRef": { "oid": "SRV-OID-001", "id": "SRV-LIC-ACT" },
    "DENARef": null,
    "SIARef": null,
    "participatingOrgUnits": [
      {
        "orgUnit": { "id": "ORG-URBANISMO", "displayNameByLanguage": { "SPANISH": "Departamento de Urbanismo" } },
        "role": "RESPONSIBLE"
      }
    ]
  },
  "procedure": {
    "serviceNameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
    "originRef": { "oid": "PROC-OID-001", "id": "PROC-LIC-APER" },
    "DENARef": null,
    "SIARef": null
  }
}
```

---

## Jerarquía

```
Servicio Administrativo
 └── Procedimiento
      └── Expediente (1:N)
           ├── Notificaciones
           ├── Registros oficiales
           └── Pagos
```

> **Nota:** Las citas (`scheduleItem`) NO pertenecen a esta jerarquía. Son objetos independientes.

---

## Notas para la administración

- `originRef.id` es obligatorio: la administración debe proporcionar al menos el identificador de negocio del servicio y procedimiento en su catálogo propio.
- `DENARef` y `SIARef` son opcionales. Si la administración conoce el código DENA o SIA, puede incluirlo para facilitar la correlación.
- `serviceNameByLanguage` debe incluir al menos textos en `SPANISH` y `BASQUE`.
