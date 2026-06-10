# 📎 Adjuntos

Recursos complementarios a la documentación técnica de DENA para facilitar la integración y desarrollo por parte de las administraciones.

---

## 🚀 Colecciones Postman

**Directorio:** [`postman/`](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos/postman)

Colecciones y environments Postman organizados por funcionalidad para facilitar las **pruebas de integración** con DENA.

### Disponibles

| Archivo | Tipo | Descripción |
|---------|------|-------------|
| [`DENA ADMINS.postman_collection.json`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/postman/DENA%20ADMINS.postman_collection.json) | Collection | Colección principal para administraciones |
| [`DENA-PRE-ADMINS.postman_environment.json`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/postman/DENA-PRE-ADMINS.postman_environment.json) | Environment | Configuración de entorno pre-producción |

### Funcionalidades cubiertas

- ✅ **Data-Retrieve** — Consulta de datos de personas
- ✅ **Metadata-Sync** — Sincronización de metadatos
- ✅ **Person-Sync** — Sincronización de personas
- ✅ **Autenticación OAuth2** — Flujos de autenticación
- ✅ **Environments** — Configuraciones para diferentes entornos

### Uso recomendado

1. **Importar collection y environment** en Postman
2. **Configurar variables** de entorno (URLs, credenciales)
3. **Ejecutar requests** para validar integración
4. **Usar como referencia** para implementación

---

## 🖼️ Imágenes y Diagramas

**Directorio:** `imagenes/`

Recursos visuales utilizados en la documentación técnica.

### Diagramas de Arquitectura

| Archivo | Formato | Descripción |
|---------|---------|-------------|
| [`DENA_Architecture.drawio`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/drawio/DENA_Architecture.drawio) | Draw.io | Diagrama editable de arquitectura general |
| [`DENA_Architecture.png`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/DENA_Architecture.png) | PNG | Exportación del diagrama de arquitectura |

### Diagramas de Autenticación

| Archivo | Formato | Descripción |
|---------|---------|-------------|
| [`login-giltza.drawio`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/drawio/login-giltza.drawio) | Draw.io | Flujo de login con Giltza (editable) |
| [`login-giltza.png`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/login-giltza.png) | PNG | Exportación del flujo Giltza |
| [`webauthn-login.drawio`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/drawio/webauthn-login.drawio) | Draw.io | Flujo de login WebAuthN (editable) |
| [`webauthn-login.png`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/webauthn-login.png) | PNG | Exportación del flujo WebAuthN login |
| [`webauthn-register.drawio`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/drawio/webauthn-register.drawio) | Draw.io | Flujo de registro WebAuthN (editable) |
| [`webauthn-register.png`](https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/adjuntos/imagenes/webauthn-register.png) | PNG | Exportación del flujo WebAuthN register |

### Formatos disponibles

- 📝 **`.drawio`** — Archivos editables con [Draw.io](https://app.diagrams.net/)
- 🖼️ **`.png`** — Exportaciones para visualización directa

---

## 📁 Estructura de directorios

```
adjuntos/
├── postman/                           # Colecciones Postman
│   ├── DENA ADMINS.postman_collection.json
│   └── DENA-PRE-ADMINS.postman_environment.json
│
├── imagenes/                          # Recursos visuales
│   ├── drawio/                        # Archivos editables
│   │   ├── DENA_Architecture.drawio
│   │   ├── login-giltza.drawio
│   │   ├── webauthn-login.drawio
│   │   └── webauthn-register.drawio
│   │
│   ├── DENA_Architecture.png          # Exportaciones PNG
│   ├── login-giltza.png
│   ├── webauthn-login.png
│   └── webauthn-register.png
│
└── index.md                           # Este archivo
```

---

## 🔗 Acceso público

Todos los recursos están disponibles públicamente en:

**🌐 GitHub:** [DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos)

- **Descarga directa** de archivos individuales
- **Visualización online** de imágenes
- **Clonado del repositorio** completo

---

## 📋 Guías de uso

### Para desarrolladores
1. **Postman** → Importar collections para pruebas de API
2. **Diagramas** → Consultar arquitectura y flujos
3. **Environments** → Configurar entornos de desarrollo/pruebas

### Para arquitectos
1. **Draw.io** → Editar y actualizar diagramas
2. **PNG** → Incluir diagramas en documentación
3. **Arquitectura** → Comprender la estructura del sistema

### Para administraciones
1. **Collections** → Validar integración con DENA
2. **Environments** → Configurar credenciales y URLs
3. **Documentación visual** → Entender flujos y procesos


<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
