# :material-file-code: Ejemplos de código

Proyecto Java de referencia con ejemplos de integración para administraciones.

---

!!! warning "En desarrollo"

    El proyecto de ejemplos está en proceso de elaboración. Se publicará en próximas versiones.

---

## Contenido previsto

| Ejemplo | Semántica | Descripción |
|---|---|---|
| Data-Retrieve básico | DATA-RETRIEVE | Endpoint mínimo que devuelve expedientes |
| Metadata-Sync | METADATA-SYNC | Notificación de cambios a DENA |
| Person-Sync Pull | PERSON-SYNC | Descarga de fichero de personas |
| Person-Sync Push | PERSON-SYNC | Recepción de notificaciones de personas |
| Autenticación OAuth2 | Autenticación | Obtención y uso de tokens |

---

## Requisitos para ejecutar los ejemplos

| Herramienta | Versión |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-apachemaven: Maven | 3.9+ |
| :material-connection: Conectividad | Hacia DENA PRE |

---

## Cómo usar (cuando esté disponible)

```bash
git clone <url-repositorio-ejemplos>
cd dena-data-codesamples
mvn clean package
java -jar target/<artefacto>.jar
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
