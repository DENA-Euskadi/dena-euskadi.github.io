# 📐 Semántica para las Administraciones

Definición semántica de los objetos y servicios intercambiados entre **CORE DENA** y las **Administraciones Públicas**.

---

## Diagrama general

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#000000"
    primaryBorderColor: "#CCCCCC"
    lineColor: "#4A4A4A"
    fontSize: "13px"
---
flowchart TD
    BASE["<b>Semántica Base</b><br/><i>Objetos comunes compartidos</i>"]

    BASE --> DR["<b>DATA-RETRIEVE</b><br/><i>Consulta de datos<br/>de persona ciudadana</i>"]
    BASE --> DS["<b>METADATA-SYNC</b><br/><i>Sincronización de metadatos<br/>entre DENA y administraciones</i>"]
    BASE --> PS["<b>PERSON-SYNC</b><br/><i>Sincronización de<br/>personas</i>"]

    style BASE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:8,ry:8
    style DR fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style DS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:8,ry:8
    style PS fill:#d5e8d4,stroke:#82b366,color:#000000,rx:8,ry:8

    click DR "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/data-retrieve/index.md" "Ver DATA-RETRIEVE"
    click DS "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/metadata-sync/index.md" "Ver METADATA-SYNC"
    click PS "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/person-sync/index.md" "Ver PERSON-SYNC"
    click BASE "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/semantica-base/index.md" "Ver Semántica Base"
```

| Color | Significado |
|-------|-------------|
| 🟣 Violeta | Modelo base compartido |
| 🟠 Naranja | DATA-RETRIEVE (consulta) |
| 🔵 Azul claro | METADATA-SYNC (sincronización de metadatos) |
| 🟢 Verde | PERSON-SYNC (sincronización de personas) |

---

## 📑 Contenido

### Semántica Base

Objetos y definiciones compartidas por todas las semánticas.

| Documento | Descripción |
|-----------|-------------|
| [Índice](./semantica-base/index.md) | Modelo base |
| [Modelo](./semantica-base/modelo/index.md) | Definición de objetos base |

---

### DATA-RETRIEVE

Mecanismo mediante el cual DENA solicita datos de una persona ciudadana a la administración.

| Documento | Descripción |
|-----------|-------------|
| [Índice](./data-retrieve/index.md) | Visión general, flujo y modelo de datos |
| [Endpoint](./data-retrieve/endpoint-data-retrieve.md) | Contrato REST: request, response, códigos HTTP |
| [Validaciones](./data-retrieve/validaciones.md) | Reglas de formato, campos obligatorios, coherencia |
| [Errores](./data-retrieve/errores-troubleshooting.md) | Guía de errores comunes y resolución |
| [Snippets](./data-retrieve/snippets-codigo.md) | Implementación en Java, C#, Python, Node.js, PHP |

**Objetos de datos:**

| Documento | Descripción |
|-----------|-------------|
| [Campos comunes](./data-retrieve/data/campos-comunes.md) | Campos heredados (oid, id, urls, refs) |
| [Expediente](./data-retrieve/data/expediente.md) | Expediente administrativo |
| [Notificación](./data-retrieve/data/notificacion.md) | Notificación / comunicación oficial |
| [Registro Oficial](./data-retrieve/data/registro-oficial.md) | Asiento registral |
| [Pago](./data-retrieve/data/pago.md) | Pago único y domiciliación |
| [Cita](./data-retrieve/data/cita.md) | Cita previa / elemento de agenda |
| [Servicio Administrativo](./data-retrieve/data/servicio-administrativo.md) | Servicio y procedimiento |
| [Unidad Orgánica](./data-retrieve/data/unidad-organica.md) | Unidad organizativa |

---

### METADATA-SYNC

Sincronización de datos entre DENA y las administraciones.

| Documento | Descripción |
|-----------|-------------|
| [Índice](./metadata-sync/index.md) | Visión general |
| [Endpoint](./semantica/metadata-sync/endpoint-sync-metadata.md) | Contrato REST del endpoint |

---

### PERSON-SYNC

Sincronización de datos de personas entre DENA y las administraciones.

| Documento | Descripción |
|-----------|-------------|
| [Índice](./person-sync/index.md) | Visión general |
| [Push](./person-sync/modelo/push.md) | Modelo push (DENA → administración) |
| [Pull](./person-sync/modelo/pull.md) | Modelo pull (administración → DENA) |

---

### Otra documentación

| Documento | Descripción |
|-----------|-------------|
| [Otra documentación](./otra-documentacion.md) | Documentación complementaria |









<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
