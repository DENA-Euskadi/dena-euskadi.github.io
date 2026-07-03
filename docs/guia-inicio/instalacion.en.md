# :material-cog: Installation and environment setup

Guide to prepare your development environment and work with the DENA repositories.

---

## Prerequisites

| Tool | Minimum version | Purpose |
|---|---|---|
| :fontawesome-brands-java: JDK | {{ dena.java_version }} | Compilation and execution |
| :simple-apachemaven: Maven | {{ dena.maven_version }} | Dependency management and build |
| :material-git: Git | 2.x | Version control |
| :material-application-brackets: IDE | IntelliJ / Eclipse / VS Code | Development |
| :material-docker: Docker (optional) | 20+ | Run auxiliary services (Keycloak, mocks) |

---

## 1. Clone the repositories

!!! warning "Version selection"
    
    It is essential to verify and select the appropriate repository version according to the target environment. Use the version tags corresponding to the environment where you will deploy. Check the [compatibility matrix](../referencia/matriz-compatibilidad.md) to determine the correct version.

```bash
# Main documentation repository
git clone {{ repos.docs_clone }}
cd dena-common-docs
git checkout v0.3.26

# Connectivity test (to validate your environment)
git clone {{ repos.conx_test_clone }}
cd dena-admin-conx-test
git checkout v1.0.0
```

---

## 2. Configure Maven

Create or update your `~/.m2/settings.xml` (or `settings-dena.xml`) with the required repositories:

```xml
<settings>
  <profiles>
    <profile>
      <id>dena</id>
      <repositories>
        <repository>
          <id>maven-central</id>
          <url>https://repo.maven.apache.org/maven2</url>
        </repository>
        <repository>
          <id>spring-milestones</id>
          <url>https://repo.spring.io/milestone</url>
        </repository>
      </repositories>
    </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>dena</activeProfile>
  </activeProfiles>
</settings>
```

!!! info "EJIE internal repositories"

    If you have access to EJIE/Nexus internal repositories, also add:

    ```xml
    <repository>
      <id>ejie-releases</id>
      <url>https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-releases/</url>
    </repository>
    <repository>
      <id>ejie-snapshots</id>
      <url>https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-snapshots/</url>
    </repository>
    ```

---

## 3. Build the connectivity test project

```bash
cd dena-admin-conx-test
mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc
mvn package -Pstandalone
```

!!! success "✓ Environment correct"

    If the build finishes with `BUILD SUCCESS`, your environment is correctly configured.

!!! failure "✗ Common failures"

    - **JDK not found:** Verify that `JAVA_HOME` points to JDK {{ dena.java_version }}
    - **Unresolved dependencies:** Check that `settings.xml` has the correct repositories
    - **Maven not found:** Make sure `mvn` is in the `PATH`

---

## 4. Verify connectivity with DENA

```bash
java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-v1.0.0.war
```

Open another terminal and run:

```bash
curl http://localhost:8082/api/hello
```

!!! example "Expected response"

    ```json
    {
      "invocationResult": "Hello TestAdmin, welcome to DENA standard!"
    }
    ```

---

## 5. IDE Configuration

=== ":material-code-tags: IntelliJ IDEA"

    1. **File > Open** → Select the project directory
    2. IntelliJ will detect the `pom.xml` and import it as a Maven project
    3. Configure JDK {{ dena.java_version }}: **File > Project Structure > SDKs** → Add JDK {{ dena.java_version }}
    4. Enable Lombok: **Settings > Build > Compiler > Annotation Processors** → ✓ Enable annotation processing

=== ":material-application: Eclipse"

    1. **File > Import > Maven > Existing Maven Projects**
    2. Select the project root directory
    3. Install Lombok plugin if not present: [projectlombok.org/setup/eclipse](https://projectlombok.org/setup/eclipse)
    4. If there are AspectJ projects:
        - Expand `src/main/aspect`
        - Right-click on the `aspect` folder → **Build Path > Use as Source Folder**
        - Right-click on the project → **Configure > Convert to AspectJ Project**

=== ":material-microsoft-visual-studio-code: VS Code"

    1. Install extensions: **Extension Pack for Java** + **Lombok Annotations Support**
    2. Open the project folder
    3. VS Code will automatically detect the `pom.xml`
    4. Verify that `java.configuration.runtimes` points to JDK {{ dena.java_version }} in `settings.json`

---

## :material-arrow-right-circle: Next step

<div class="grid cards" markdown>

-   :material-connection:{ .lg .middle } **Test communications**

    ---

    Validate bidirectional connectivity between your infrastructure and DENA.

    [:octicons-arrow-right-24: Test communications](./probar-comunicaciones.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
