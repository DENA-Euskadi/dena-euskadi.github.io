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

!!! warning "Control de versiones en repositorios DENA"
    
    Al clonar cualquier repositorio de DENA, asegúrese de utilizar la etiqueta de versión apropiada mediante `git checkout <tag>` para garantizar la compatibilidad con su entorno de desarrollo y despliegue.

```bash
git clone <url-repositorio-ejemplos>
cd dena-data-codesamples
mvn clean package
java -jar target/<artefacto>.jar
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
