# :material-cog: Instalación y configuración del entorno

Guía para preparar tu entorno de desarrollo y poder trabajar con los repositorios DENA.

---

## Requisitos previos

| Herramienta | Versión mínima | Propósito |
|---|---|---|
| :fontawesome-brands-java: JDK | {{ dena.java_version }} | Compilación y ejecución |
| :simple-apachemaven: Maven | {{ dena.maven_version }} | Gestión de dependencias y build |
| :material-git: Git | 2.x | Control de versiones |
| :material-application-brackets: IDE | IntelliJ / Eclipse / VS Code | Desarrollo |
| :material-docker: Docker (opcional) | 20+ | Levantar servicios auxiliares (Keycloak, mocks) |

---

## 1. Clonar los repositorios

!!! warning "Selección de versión"
    
    Es fundamental verificar y seleccionar la versión adecuada del repositorio según el entorno de destino. Utilice las etiquetas de versión (tags) correspondientes al entorno donde realizará el despliegue. Consulte la [matriz de compatibilidad](../referencia/matriz-compatibilidad.md) para determinar la versión correcta.

```bash
# Repositorio principal de documentación
git clone {{ repos.docs_clone }}
cd dena-common-docs
git checkout v0.3.26

# Test de conectividad (para validar tu entorno)
git clone {{ repos.conx_test_clone }}
cd dena-admin-conx-test
git checkout v1.0.0
```

---

## 2. Configurar Maven

Crea o actualiza tu `~/.m2/settings.xml` (o `settings-dena.xml`) con los repositorios necesarios:

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

!!! info "Repositorios internos EJIE"

    Si tienes acceso a los repositorios internos EJIE/Nexus, añade también:

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

## 3. Compilar el proyecto de test de conectividad

```bash
cd dena-admin-conx-test
mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc
mvn package -Pstandalone
```

!!! success "✓ Entorno correcto"

    Si la compilación termina con `BUILD SUCCESS`, tu entorno está correctamente configurado.

!!! failure "✗ Fallos comunes"

    - **JDK no encontrado:** Verifica que `JAVA_HOME` apunta a JDK {{ dena.java_version }}
    - **Dependencias no resueltas:** Revisa que `settings.xml` tiene los repositorios correctos
    - **Maven no encontrado:** Asegúrate de que `mvn` está en el `PATH`

---

## 4. Verificar la conectividad con DENA

```bash
java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-v1.0.0.war
```

Abre otra terminal y ejecuta:

```bash
curl http://localhost:8082/api/hello
```

!!! example "Respuesta esperada"

    ```json
    {
      "invocationResult": "Hello TestAdmin, welcome to DENA standard!"
    }
    ```

---

## 5. Configuración del IDE

=== ":material-code-tags: IntelliJ IDEA"

    1. **File > Open** → Seleccionar el directorio del proyecto
    2. IntelliJ detectará el `pom.xml` y lo importará como proyecto Maven
    3. Configurar JDK {{ dena.java_version }}: **File > Project Structure > SDKs** → Añadir JDK {{ dena.java_version }}
    4. Habilitar Lombok: **Settings > Build > Compiler > Annotation Processors** → ✓ Enable annotation processing

=== ":material-application: Eclipse"

    1. **File > Import > Maven > Existing Maven Projects**
    2. Seleccionar el directorio raíz del proyecto
    3. Instalar plugin Lombok si no está presente: [projectlombok.org/setup/eclipse](https://projectlombok.org/setup/eclipse)
    4. Si hay proyectos AspectJ:
        - Expandir `src/main/aspect`
        - Click derecho en la carpeta `aspect` → **Build Path > Use as Source Folder**
        - Click derecho en el proyecto → **Configure > Convert to AspectJ Project**

=== ":material-microsoft-visual-studio-code: VS Code"

    1. Instalar extensiones: **Extension Pack for Java** + **Lombok Annotations Support**
    2. Abrir la carpeta del proyecto
    3. VS Code detectará automáticamente el `pom.xml`
    4. Verificar que `java.configuration.runtimes` apunta a JDK {{ dena.java_version }} en `settings.json`

---

## :material-arrow-right-circle: Siguiente paso

<div class="grid cards" markdown>

-   :material-connection:{ .lg .middle } **Probar comunicaciones**

    ---

    Valida la conectividad bidireccional entre tu infraestructura y DENA.

    [:octicons-arrow-right-24: Probar comunicaciones](./probar-comunicaciones.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
