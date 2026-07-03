# :material-file-code: Code Examples

Java reference project with integration examples for administrations.

---

!!! warning "Under development"

    The examples project is being prepared. It will be published in upcoming versions.

---

## Planned content

| Example | Semantics | Description |
|---|---|---|
| Basic Data-Retrieve | DATA-RETRIEVE | Minimal endpoint that returns records |
| Metadata-Sync | METADATA-SYNC | Change notification to DENA |
| Person-Sync Pull | PERSON-SYNC | Persons file download |
| Person-Sync Push | PERSON-SYNC | Person notifications reception |
| OAuth2 Authentication | Authentication | Token obtention and usage |

---

## Requirements to run examples

| Tool | Version |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-apachemaven: Maven | 3.9+ |
| :material-connection: Connectivity | Towards DENA PRE |

---

## How to use (when available)

!!! warning "Version control in DENA repositories"
    
    When cloning any DENA repository, make sure to use the appropriate version tag via `git checkout <tag>` to ensure compatibility with your development and deployment environment.

```bash
git clone <examples-repository-url>
cd dena-data-codesamples
mvn clean package
java -jar target/<artifact>.jar
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
