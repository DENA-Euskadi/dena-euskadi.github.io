# 📘 DENA — Documentación para Administraciones

> **Versión:** `v0.3.25`  
> **Fecha de publicación:** 2026-06-10  
> **Repositorio:** [DENA-Euskadi/dena-common-docs](https://github.com/DENA-Euskadi/dena-common-docs)

---

## Descripción

Documentación técnica y funcional del proyecto **DENA** dirigida a las administraciones públicas que se integran con la plataforma de interoperabilidad.

---

## 📑 Índice de contenido

### 🏗️ Arquitectura

| Documento | Descripción |
|-----------|-------------|
| [Índice](./arquitectura/index.md) | Visión general de la arquitectura |
| [Diagramas](./arquitectura/diagramas.md) | Diagramas de arquitectura del sistema |
| [Otra documentación](./arquitectura/otra-documentacion.md) | Documentación complementaria |

### 🔐 Autenticación

| Documento | Descripción |
|-----------|-------------|
| [Índice](./autenticacion/index.md) | Visión general de autenticación |
| [Cliente → CORE DENA](./autenticacion/client-core-dena.md) | Autenticación del cliente contra DENA |
| [Administración → CORE DENA](./autenticacion/administracion-core-dena/index.md) | Autenticación de la administración contra DENA |
| [CORE DENA → Administración](./autenticacion/core-dena-administracion/index.md) | Autenticación de DENA contra la administración |

### 📐 Semántica para Administraciones

| Documento | Descripción |
|-----------|-------------|
| [Índice](./semantica/index.md) | Visión general del modelo semántico |
| **Data-Retrieve** | |
| [↳ Índice Data-Retrieve](./semantica/data-retrieve/index.md) | Mecanismo de consulta de datos |
| [↳ Endpoint](./semantica/data-retrieve/endpoint-data-retrieve.md) | Contrato REST del endpoint |
| [↳ Validaciones](./semantica/data-retrieve/validaciones.md) | Reglas de formato y validación |
| [↳ Errores](./semantica/data-retrieve/errores-troubleshooting.md) | Guía de errores y troubleshooting |
| [↳ Snippets de código](./semantica/data-retrieve/snippets-codigo.md) | Implementación en Java, C#, Python, Node.js, PHP |
| **Objetos de datos** | |
| [↳ Campos comunes](./semantica/data-retrieve/data/campos-comunes.md) | Campos heredados por todos los objetos |
| [↳ Expediente](./semantica/data-retrieve/data/expediente.md) | Expediente administrativo |
| [↳ Notificación](./semantica/data-retrieve/data/notificacion.md) | Notificación / comunicación oficial |
| [↳ Registro Oficial](./semantica/data-retrieve/data/registro-oficial.md) | Asiento registral |
| [↳ Pago](./semantica/data-retrieve/data/pago.md) | Pago único y domiciliación |
| [↳ Cita](./semantica/data-retrieve/data/cita.md) | Cita previa / elemento de agenda |
| [↳ Servicio Administrativo](./semantica/data-retrieve/data/servicio-administrativo.md) | Servicio y procedimiento |
| [↳ Unidad Orgánica](./semantica/data-retrieve/data/unidad-organica.md) | Unidad organizativa |
| **Metadata-Sync** | |
| [↳ Índice Metadata-Sync](./semantica/metadata-sync/index.md) | Sincronización de datos |
| [↳ Endpoint](./semantica/metadata-sync/endpoint-sync-metadata.md) | Contrato REST del endpoint |
| **Person-Sync** | |
| [↳ Índice Person-Sync](./semantica/person-sync/index.md) | Sincronización de personas |
| [↳ Pull](./semantica/person-sync/pull.md) | Modelo pull (administración → DENA) |
| [↳ Push](./semantica/person-sync/push.md) | Modelo push (DENA → administración) |
| **Semántica Base** | |
| [↳ Índice](./semantica/semantica-base/index.md) | Modelo semántico base |

### 🔧 DevTools

| Herramienta | Descripción |
|-------------|-------------|
| [Índice](./devtools/index.md) | Herramientas de desarrollo para pruebas e integración |
| [DENA DevTools Services](./devtools/index.md) | Herramienta web para testing de endpoints HTTP desde infraestructura DENA |
| [DENA Admin Connection Test](https://github.com/DENA-Euskadi/dena-admin-conx-test) | Herramienta para realizar conexiones de prueba con DENA |

### 💻 Ejemplos de código

| Documento | Descripción |
|-----------|-------------|
| [Índice](./ejemplos-codigo/index.md) | Proyecto Java de ejemplos |

### 📎 Adjuntos

| Documento | Descripción |
|-----------|-------------|
| [Índice](./adjuntos/index.md) | Colecciones Postman, imágenes y recursos |
| [Colecciones Postman](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos/postman) | Colecciones y environments para facilitar las pruebas de integración |
| [→ Metadata-Sync](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos/postman) | Colección para sincronización de metadatos |
| [→ Person-Sync](https://github.com/DENA-Euskadi/dena-common-docs/tree/main/docs/adjuntos/postman) | Colección para sincronización de personas |

---

## 📦 Dependencias

### Framework Base

| Librería | Versión | Propósito |
|----------|---------|----------|
| **Java** | `21+` | Versión mínima del JDK para toda la plataforma DENA |
| **R01F Framework** | `0.3.25-SNAPSHOT` | Framework base con `OIDBaseMutable`, `LanguageTexts`, utilidades y servicios |
| **Spring Framework** | `6.1.0` | Framework de aplicaciones empresariales |
| **Jackson** | `2.15.2` | Serialización/deserialización JSON |

### Librerías de Desarrollo

| Librería | Versión | Propósito |
|----------|---------|----------|
| **Lombok** | `1.18.34` | Simplificación de código con anotaciones (`@Getter`, `@Setter`, `@Accessors`) |
| **SLF4J** | `2.0.9` | API de logging |
| **JUnit 5** | `5.x` | Framework de pruebas unitarias |
| **Mockito** | Latest | Framework de mocking para tests |

### Maven Plugins

| Plugin | Versión | Propósito |
|--------|---------|----------|
| `maven-compiler-plugin` | `3.8.1` | Compilación de fuentes Java 21 |
| `maven-surefire-plugin` | `3.0.0-M3` | Ejecución de tests unitarios |
| `flatten-maven-plugin` | `1.1.0` | Gestión de versiones con `${revision}` |
| `build-helper-maven-plugin` | `1.7` | Inclusión de recursos adicionales |
| `versions-maven-plugin` | `2.7` | Gestión de versiones de dependencias |

### Repositorios Maven

| Repositorio | URL | Propósito |
|-------------|-----|----------|
| **Maven Central** | `https://repo.maven.apache.org/maven2` | Dependencias públicas estándar |
| **Spring Milestones** | `https://repo.spring.io/milestone` | Dependencias de Spring Framework |
| **EJIE Releases** | `https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-releases/` | Artefactos DENA y R01F estables |
| **EJIE Snapshots** | `https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-snapshots/` | Artefactos DENA y R01F en desarrollo |
| **EJIE Group** | `https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-group/` | Agregador de repositorios EJIE |

### Profiles Maven

- **Default Profile** - Para desarrollo local con versiones `SNAPSHOT`
- **batsdlc Profile** - Para compilaciones en GitLab CI/CD con versiones específicas

---

## Sobre esta documentación

Este repositorio recoge la documentación pública de DENA orientada a las administraciones que se integran con la plataforma:

- **Arquitectura** — Diagramas y documentación de la estructura del sistema
- **Autenticación** — Flujos OAuth2 entre los distintos actores (Cliente, CORE DENA, Administraciones)
- **Semántica** — Modelo de datos, endpoints REST, validaciones y snippets de código
- **DevTools** — Herramientas de desarrollo para facilitar las pruebas e integración con DENA
- **Ejemplos de código** — Proyecto de referencia para implementar la integración
- **Adjuntos** — Colecciones y environments Postman para facilitar las pruebas de integración, imágenes y recursos complementarios

---

## Histórico de versiones

| Versión | Fecha | Descripción |
|---------|-------|-------------|
| `v0.3.25` | 2026-06-10 | Documentación DATA-RETRIEVE, PERSON-SYNC, DATA-SYNC |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
