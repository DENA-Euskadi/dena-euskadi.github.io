# :material-cog: Instalazioa eta ingurunearen konfigurazioa

Zure garapen-ingurunea prestatzeko eta DENAren biltegiekin lan egiteko gida.

---

## Aurrebaldintzak

| Tresna | Gutxieneko bertsioa | Helburua |
|---|---|---|
| :fontawesome-brands-java: JDK | {{ dena.java_version }} | Konpilazioa eta exekuzioa |
| :simple-apachemaven: Maven | {{ dena.maven_version }} | Dependentzien kudeaketa eta build |
| :material-git: Git | 2.x | Bertsio kontrola |
| :material-application-brackets: IDE | IntelliJ / Eclipse / VS Code | Garapena |
| :material-docker: Docker (aukerakoa) | 20+ | Zerbitzu lagungarriak altxatu (Keycloak, mocks) |

---

## 1. Biltegiak klonatu

!!! warning "Bertsioaren hautaketa"
    
    Funtsezkoa da biltegiaren bertsio egokia egiaztatu eta hautatzea helburu-ingurunearen arabera. Erabili hedapena egingo duzun inguruneari dagozkion bertsio etiketak (tags). Kontsultatu [bateragarritasun matrizea](../referencia/matriz-compatibilidad.md) bertsio zuzena zehazteko.

```bash
# Dokumentazioaren biltegi nagusia
git clone {{ repos.docs_clone }}
cd dena-common-docs
git checkout v0.3.26

# Konektibitate testa (zure ingurunea baliozkotu)
git clone {{ repos.conx_test_clone }}
cd dena-admin-conx-test
git checkout v1.0.0
```

---

## 2. Maven konfiguratu

Sortu edo eguneratu zure `~/.m2/settings.xml` (edo `settings-dena.xml`) beharrezko biltegiekin:

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

!!! info "EJIE barneko biltegiak"

    EJIE/Nexus barneko biltegietarako sarbidea baduzu, gehitu ere:

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

## 3. Konektibitate test proiektua konpilatu

```bash
cd dena-admin-conx-test
mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc
mvn package -Pstandalone
```

!!! success "✓ Ingurune zuzena"

    Konpilazioak `BUILD SUCCESS`-ekin amaitzen badu, zure ingurunea ondo konfiguratuta dago.

!!! failure "✗ Hutsegite arruntak"

    - **JDK ez da aurkitu:** Egiaztatu `JAVA_HOME`-k JDK {{ dena.java_version }}-ra apuntatzen duela
    - **Ebatzi gabeko dependentziak:** Egiaztatu `settings.xml`-ek biltegi zuzenak dituela
    - **Maven ez da aurkitu:** Ziurtatu `mvn` `PATH`-ean dagoela

---

## 4. DENArekiko konektibitatea egiaztatu

```bash
java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-v1.0.0.war
```

Ireki beste terminal bat eta exekutatu:

```bash
curl http://localhost:8082/api/hello
```

!!! example "Espero den erantzuna"

    ```json
    {
      "invocationResult": "Hello TestAdmin, welcome to DENA standard!"
    }
    ```

---

## 5. IDE Konfigurazioa

=== ":material-code-tags: IntelliJ IDEA"

    1. **File > Open** → Proiektuaren direktorioa hautatu
    2. IntelliJ-k `pom.xml` detektatuko du eta Maven proiektu gisa inportatuko du
    3. JDK {{ dena.java_version }} konfiguratu: **File > Project Structure > SDKs** → JDK {{ dena.java_version }} gehitu
    4. Lombok gaitu: **Settings > Build > Compiler > Annotation Processors** → ✓ Enable annotation processing

=== ":material-application: Eclipse"

    1. **File > Import > Maven > Existing Maven Projects**
    2. Proiektuaren erro direktorioa hautatu
    3. Lombok plugina instalatu ez badago: [projectlombok.org/setup/eclipse](https://projectlombok.org/setup/eclipse)
    4. AspectJ proiektuak badaude:
        - `src/main/aspect` zabaldu
        - Eskuineko klika `aspect` karpetan → **Build Path > Use as Source Folder**
        - Eskuineko klika proiektuan → **Configure > Convert to AspectJ Project**

=== ":material-microsoft-visual-studio-code: VS Code"

    1. Luzapenak instalatu: **Extension Pack for Java** + **Lombok Annotations Support**
    2. Proiektuaren karpeta ireki
    3. VS Code-k automatikoki detektatuko du `pom.xml`
    4. Egiaztatu `java.configuration.runtimes`-ek JDK {{ dena.java_version }}-ra apuntatzen duela `settings.json`-en

---

## :material-arrow-right-circle: Hurrengo urratsa

<div class="grid cards" markdown>

-   :material-connection:{ .lg .middle } **Komunikazioak probatu**

    ---

    Zure azpiegituraren eta DENAren arteko norabide biko konektibitatea baliozkotu.

    [:octicons-arrow-right-24: Komunikazioak probatu](./probar-comunicaciones.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
