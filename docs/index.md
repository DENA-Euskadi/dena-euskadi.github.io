---
hide:
  - toc
---

# DENA Interop — Documentación para Administraciones

<div class="grid cards" markdown>

-   :material-rocket-launch:{ .lg .middle } **¿Primera vez aquí?**

    ---

    Checklist completo: desde instalar el entorno hasta tu primera integración funcionando.

    [:octicons-arrow-right-24: Onboarding](./guia-inicio/onboarding.md)

-   :material-swap-horizontal:{ .lg .middle } **Implementar integración**

    ---

    Endpoints estándar que tu administración debe exponer para comunicarse con DENA.

    [:octicons-arrow-right-24: Semántica](./semantica/index.md)

-   :material-shield-lock:{ .lg .middle } **Configurar autenticación**

    ---

    Flujos OAuth2 entre tu sistema y DENA (client_credentials).

    [:octicons-arrow-right-24: Autenticación](./autenticacion/index.md)

-   :material-wrench:{ .lg .middle } **Herramientas de testing**

    ---

    DevTools, Postman collections y test de conectividad bidireccional.

    [:octicons-arrow-right-24: DevTools](./devtools/index.md)

</div>

---

## ¿Qué es DENA?

**DENA** es la plataforma de interoperabilidad del Gobierno Vasco que permite a las personas usuarias acceder, desde una única aplicación, a los datos que las distintas administraciones públicas gestionan sobre ellas.

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "14px"
    fontFamily: "Manrope, sans-serif"
---
graph LR
    A[Persona usuaria] -->|App DENA| B(CORE DENA)
    B -->|Data-Retrieve| C[Administración A]
    B -->|Data-Retrieve| D[Administración B]
    C -->|Metadata-Sync| B
    D -->|Metadata-Sync| B
    B <-->|Person-Sync| C
    B <-->|Person-Sync| D
    
    style A fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px
    style B fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style C fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
    style D fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## :material-map-marker-path: ¿Qué necesitas hacer?

=== "Servir datos a DENA"

    Tu administración expone un endpoint REST para que DENA consulte datos de una persona.

    **Endpoint:** `POST /api/retrieveData`

    [:octicons-arrow-right-24: Documentación Data-Retrieve](./semantica/data-retrieve/index.md)

=== "Notificar cambios"

    Tu sistema notifica a DENA cuando hay nuevos datos disponibles para una persona.

    **Endpoint:** `POST /api/syncMetadata`

    [:octicons-arrow-right-24: Documentación Metadata-Sync](./semantica/metadata-sync/index.md)

=== "Sincronizar personas"

    Mantener actualizado el listado de personas registradas entre DENA y tu administración.

    **Mecanismos:** Pull (tú consultas) / Push (DENA te notifica)

    [:octicons-arrow-right-24: Documentación Person-Sync](./semantica/person-sync/index.md)

=== "Probar conectividad"

    Validar que la comunicación bidireccional entre tu infraestructura y DENA funciona.

    ```bash
    curl -X POST http://localhost:8082/api/conxTest \
      -H "Content-Type: application/json" \
      -d '{"environment": "PRE"}'
    ```

    [:octicons-arrow-right-24: Guía de comunicaciones](./guia-inicio/probar-comunicaciones.md)

---

## :material-lightning-bolt: Inicio rápido

!!! warning "Verificar versión del repositorio"
    
    Asegúrese de utilizar la versión correcta del repositorio antes de proceder con la clonación. La versión recomendada para el entorno de trabajo actual es la etiquetada como estable en el repositorio.

!!! tip "5 minutos para validar tu entorno"

    ```bash
    # 1. Clonar el test de conectividad
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test

    # 2. Compilar y arrancar
    mvn clean package -Pstandalone
    java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war

    # 3. Verificar
    curl http://localhost:8082/api/hello

    # 4. Test contra DENA PRE
    curl -X POST http://localhost:8082/api/conxTest \
      -H "Content-Type: application/json" \
      -d '{"environment": "PRE"}'
    ```

---

## :material-sitemap: Mapa de documentación

| Sección | Contenido |
|---|---|
| [:material-play-circle: Guía de inicio](./guia-inicio/onboarding.md) | Onboarding, instalación, comunicaciones, mock |
| [:material-cube-outline: Arquitectura](./arquitectura/index.md) | Visión general, diagramas, módulos del sistema |
| [:material-shield-lock: Autenticación](./autenticacion/index.md) | OAuth2 client_credentials, flujos Admin ↔ DENA |
| [:material-code-braces: Semántica](./semantica/index.md) | Data-Retrieve, Metadata-Sync, Person-Sync |
| [:material-wrench: DevTools](./devtools/index.md) | Herramienta web de testing HTTP desde DENA |
| [:material-book-open-variant: Referencia](./referencia/faq.md) | FAQ, Glosario, Troubleshooting, Changelog, Matriz |
| [:material-file-code: Ejemplos](./ejemplos-codigo/index.md) | Proyecto Java de referencia |
| [:material-paperclip: Adjuntos](./adjuntos/index.md) | Postman collections, environments, imágenes |

---

## :material-server-network: Stack tecnológico

| Componente | Versión |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-spring: Spring Boot | 3.3.5 |
| :simple-apachemaven: Maven | 3.9+ |
| :material-code-json: Jackson | 2.19.x |

---

!!! info "Entornos DENA"

    | Entorno | Internet | Euskalsarea |
    |---|---|---|
    | **PRE** | `https://api-batera.pre.dena.eus` | `https://api-batera.pre.batera.euskalsarea.eus` |
    | **PRO** | `https://api-batera.pro.dena.eus` | `https://api-batera.pro.batera.euskalsarea.eus` |

!!! question "Soporte técnico"
    
    Para consultas técnicas, problemas de integración o solicitud de credenciales:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

---

<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
