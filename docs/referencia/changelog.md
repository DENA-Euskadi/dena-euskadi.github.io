# :material-history: Changelog

Historial de versiones de la documentación DENA.

---

## v0.3.38 <small>— 2026-07-04</small> { #v0338 }

!!! success "Actual"

**Mejoras de documentación:**

- :material-palette: CSS reorganizado en ficheros numerados `01`–`06` por responsabilidad
- :material-refresh: `extra.css` migrado a ficheros numerados; fichero legacy vaciado
- :material-image: Logos de administraciones añadidos al footer (Ayto. Bilbao, Donosti, Vitoria, DFA, DFB, DFG, EUDEL)
- :material-format-size: Tamaño de logos del footer aumentado a 60px
- :material-bug: Corrección de variables dark mode faltantes en `01-variables.css`, `02-material-overrides.css` y `06-mermaid-diagrams.css`
- :material-bug: Corrección masiva `registry` → `register` en `registro-oficial.md` (es/en/eu)
- :material-web: Tipografía Manrope migrada de fuente local a Google Fonts

---

## v0.3.32 <small>— 2026-06-26</small> { #v0332 }

**Mejoras de documentación:**

- :material-palette: Aplicación completa de colores corporativos DENA (#1D3328)
- :material-image: Logo DENA redimensionado a 64px para mejor visibilidad
- :material-view-grid: Página de entrada "Operativas DENA" con navegación visual
- :material-cogs: Consolidación de secciones operativas en menú principal
- :material-email: Integración de contacto de soporte (admin-digital-data-dena@ejie.eus)
- :material-alert: Avisos de versión de repositorio en toda la documentación
- :material-star: Consistencia visual con iconos Material Design
- :material-navigation: Navegación simplificada sin emojis
- :material-help: Página de contacto y soporte dedicada

---

## v0.3.31 <small>— 2026-06-25</small> { #v0331 }

**Mejoras de documentación:**

- :material-refresh: Variables centralizadas actualizadas en `mkdocs-vars.yaml`
- :material-refresh: Todos los footers migrados a variables `{{ dena.version }}` y `{{ dena.date }}`
- :material-refresh: Headers de versión en documentación semántica usando variables
- :material-plus: Mantenibilidad mejorada: cambios de versión desde un solo archivo
- :material-bug: Corrección de inconsistencias de versionado en documentación

---

## v0.3.26 <small>— 2026-06-11</small> { #v0326 }

**Mejoras de documentación:**

- :material-plus: Página de entrada rediseñada con Material for MkDocs
- :material-plus: Sección "Guía de inicio" con instalación, comunicaciones y mock
- :material-plus: FAQ (Preguntas frecuentes)
- :material-plus: Glosario de términos
- :material-plus: Custom CSS con colores corporativos DENA
- :material-plus: Tooltips en siglas técnicas (OID, DIR3, SIA...)
- :material-plus: Variables reutilizables (`mkdocs-vars.yaml`)
- :material-refresh: Todas las páginas actualizadas con features de Material (admonitions, tabs, mermaid, grid cards)
- :material-refresh: Navegación reorganizada con tabs y secciones expandibles

---

## v0.3.25 <small>— 2026-06-10</small> { #v0325 }

**Contenido:**

- :material-plus: Documentación DATA-RETRIEVE completa (endpoint, objetos, validaciones, errores, snippets)
- :material-plus: Documentación PERSON-SYNC (Pull + Push, endpoints, modelos)
- :material-plus: Documentación METADATA-SYNC (endpoint)
- :material-plus: Guía de implementación Data-Retrieve
- :material-plus: Diagramas mermaid interactivos en semántica
- :material-plus: Colecciones Postman actualizadas

---

## v0.2.0 <small>— 2026-03-15</small> { #v020 }

**Contenido:**

- :material-plus: Documentación de autenticación (Cliente ↔ DENA, Admin ↔ DENA)
- :material-plus: Diagramas de flujo OAuth2
- :material-plus: Endpoint get-token
- :material-plus: Semántica base (estructura REST Message)

---

## v0.1.0 <small>— 2025-12-01</small> { #v010 }

**Contenido:**

- :material-plus: Estructura inicial del repositorio
- :material-plus: Arquitectura general (diagrama draw.io)
- :material-plus: Primeros modelos de datos (DataTypeRef, PersonRef, OrgAdminRef)
- :material-plus: DevTools - DENA Admin Connection Test

---

## Convención de versiones

| Formato | Significado |
|---|---|
| `v0.X.Y` | Fase de desarrollo pre-release |
| :material-plus: | Contenido nuevo |
| :material-refresh: | Contenido actualizado |
| :material-minus: | Contenido eliminado |
| :material-bug: | Corrección de errores |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
