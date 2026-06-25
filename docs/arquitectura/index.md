# :material-cube-outline: Arquitectura

## Visión General

DENA Interop es el sistema que permite la interoperabilidad con las distintas administraciones para facilitar a las personas usuarias, a través de la aplicación cliente, el acceso a los datos que las administraciones gestionan sobre ellas.

![Diagrama de arquitectura](../adjuntos/imagenes/DENA_Architecture.png)

---

## Módulos principales

<div class="grid cards" markdown>

-   :material-sync:{ .lg .middle } **Metadata Sync**

    ---

    Recibe notificaciones de las administraciones cuando hay cambios en datos de una persona. DENA almacena solo la fecha de última actualización por combinación persona + tipo de dato + administración.

    [:octicons-arrow-right-24: Semántica Metadata-Sync](../semantica/metadata-sync/index.md)

-   :material-database-arrow-right:{ .lg .middle } **Data Retrieve**

    ---

    Cuando la app cliente detecta cambios, solicita a la administración (a través de DENA) la descarga de los datos actualizados. La administración expone un endpoint estándar.

    [:octicons-arrow-right-24: Semántica Data-Retrieve](../semantica/data-retrieve/index.md)

-   :material-account-sync:{ .lg .middle } **Person Sync**

    ---

    Sincroniza el listado de personas registradas en DENA con las administraciones. Dos mecanismos: **Pull** (la administración consulta) y **Push** (DENA notifica).

    [:octicons-arrow-right-24: Semántica Person-Sync](../semantica/person-sync/index.md)

</div>

---

## Flujo general

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "13px"
    fontFamily: "Manrope, sans-serif"
    actorBkg: "#f5d836"
    actorBorder: "#1a4d1f"
    actorTextColor: "#1a4d1f"
    activationBkgColor: "#70d680"
    activationBorderColor: "#1a4d1f"
    sequenceNumberColor: "#1a4d1f"
---
sequenceDiagram
    participant App as App Cliente
    participant DENA as CORE DENA
    participant Admin as Administración

    Note over Admin,DENA: 1. Metadata Sync
    Admin->>DENA: POST /syncMetadata (hay cambios)
    DENA-->>Admin: 200 OK

    Note over App,DENA: 2. App detecta cambios
    App->>DENA: Consulta metadatos
    DENA-->>App: Hay datos nuevos en Admin X

    Note over DENA,Admin: 3. Data Retrieve
    DENA->>Admin: POST /retrieveData (persona + tipo)
    Admin-->>DENA: 200 OK + datos
    DENA-->>App: Datos actualizados
```

---

## Conector estándar vs. no estándar

!!! tip "Endpoint estándar"

    Si tu administración puede implementar el endpoint REST estándar (`POST /api/retrieveData`), la integración es directa y rápida.

    [:octicons-arrow-right-24: Guía de implementación](../semantica/data-retrieve/guia-implementacion.md)

!!! info "Conector a medida"

    Si no es posible implementar el estándar, desde DENA se desarrollará un conector específico que se adapte a los medios facilitados por la administración (SOAP, ficheros, etc.).

---

## Documentación relacionada

- [Diagramas (draw.io)](./diagramas.md)
- [Otra documentación](./otra-documentacion.md)
- [ADRs (Architecture Decision Records)]({{ repos.docs_main }}/ArchitectureDecisionRecords)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
