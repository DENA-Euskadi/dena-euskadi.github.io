---
hide:
  - toc
---

# DENA Interop — Documentation for Public Administrations

<div class="grid cards" markdown>

-   :material-rocket-launch:{ .lg .middle } **First time here?**

    ---

    Complete checklist: from setting up your environment to your first working integration.

    [:octicons-arrow-right-24: Onboarding](./guia-inicio/onboarding.md)

-   :material-swap-horizontal:{ .lg .middle } **Implement integration**

    ---

    Standard endpoints your administration must expose to communicate with DENA.

    [:octicons-arrow-right-24: Semantics](./semantica/index.md)

-   :material-shield-lock:{ .lg .middle } **Configure authentication**

    ---

    OAuth2 flows between your system and DENA (client_credentials).

    [:octicons-arrow-right-24: Authentication](./autenticacion/index.md)

-   :material-wrench:{ .lg .middle } **Testing tools**

    ---

    DevTools, Postman collections and bidirectional connectivity tests.

    [:octicons-arrow-right-24: DevTools](./devtools/index.md)

</div>

---

## What is DENA?

**DENA** is the interoperability platform of the Basque Government that allows citizens to access, from a single application, the data that different public administrations manage about them.

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
    A[Citizen] -->|DENA App| B(DENA CORE)
    B -->|Data-Retrieve| C[Administration A]
    B -->|Data-Retrieve| D[Administration B]
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

## :material-map-marker-path: What do you need to do?

=== "Serve data to DENA"

    Your administration exposes a REST endpoint so DENA can query data about a person.

    **Endpoint:** `POST /api/retrieveData`

    [:octicons-arrow-right-24: Data-Retrieve Documentation](./semantica/data-retrieve/index.md)

=== "Notify changes"

    Your system notifies DENA when new data is available for a person.

    **Endpoint:** `POST /api/syncMetadata`

    [:octicons-arrow-right-24: Metadata-Sync Documentation](./semantica/metadata-sync/index.md)

=== "Synchronize persons"

    Keep the list of registered persons updated between DENA and your administration.

    **Mechanisms:** Pull (you query) / Push (DENA notifies you)

    [:octicons-arrow-right-24: Person-Sync Documentation](./semantica/person-sync/index.md)

=== "Test connectivity"

    Validate that bidirectional communication between your infrastructure and DENA works.

    ```bash
    curl -X POST http://localhost:8082/api/conxTest \
      -H "Content-Type: application/json" \
      -d '{"environment": "PRE"}'
    ```

    [:octicons-arrow-right-24: Communications guide](./guia-inicio/probar-comunicaciones.md)

---

## :material-lightning-bolt: Quick start

!!! warning "Verify repository version"
    
    Make sure to use the correct repository version before proceeding with the clone. The recommended version for the current working environment is the one tagged as stable in the repository.

!!! tip "5 minutes to validate your environment"

    ```bash
    # 1. Clone the connectivity test
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test

    # 2. Build and start
    mvn clean package -Pstandalone
    java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war

    # 3. Verify
    curl http://localhost:8082/api/hello

    # 4. Test against DENA PRE
    curl -X POST http://localhost:8082/api/conxTest \
      -H "Content-Type: application/json" \
      -d '{"environment": "PRE"}'
    ```

---

## :material-sitemap: Documentation map

| Section | Content |
|---|---|
| [:material-play-circle: Getting Started](./guia-inicio/onboarding.md) | Onboarding, installation, communications, mock |
| [:material-cube-outline: Architecture](./arquitectura/index.md) | Overview, diagrams, system modules |
| [:material-shield-lock: Authentication](./autenticacion/index.md) | OAuth2 client_credentials, Admin ↔ DENA flows |
| [:material-code-braces: Semantics](./semantica/index.md) | Data-Retrieve, Metadata-Sync, Person-Sync |
| [:material-wrench: DevTools](./devtools/index.md) | Web-based HTTP testing tool from DENA |
| [:material-book-open-variant: Reference](./referencia/faq.md) | FAQ, Glossary, Troubleshooting, Changelog, Matrix |
| [:material-file-code: Examples](./ejemplos-codigo/index.md) | Java reference project |
| [:material-paperclip: Attachments](./adjuntos/index.md) | Postman collections, environments, images |

---

## :material-server-network: Technology stack

| Component | Version |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-spring: Spring Boot | 3.3.5 |
| :simple-apachemaven: Maven | 3.9+ |
| :material-code-json: Jackson | 2.19.x |

---

!!! info "DENA Environments"

    | Environment | Internet | Euskalsarea |
    |---|---|---|
    | **PRE** | `https://api-batera.pre.dena.eus` | `https://api-batera.pre.batera.euskalsarea.eus` |
    | **PRO** | `https://api-batera.pro.dena.eus` | `https://api-batera.pro.batera.euskalsarea.eus` |

!!! question "Technical support"
    
    For technical queries, integration issues or credential requests:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

---

<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
