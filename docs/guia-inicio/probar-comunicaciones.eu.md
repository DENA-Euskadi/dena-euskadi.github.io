# :material-connection: DENA-rekiko komunikazioak probatu

Urratsez urratseko gida zure administrazioaren eta DENA-ren arteko norabide biko konektibitatea balioztatzeko.

---

## Zer balioztatzen da?

``` mermaid
sequenceDiagram
    participant Admin as Zure Administrazioa
    participant DENA as DENA PRE/PRO

    Note over Admin,DENA: 1. proba: Admin → DENA
    Admin->>DENA: POST /api/conxTest
    DENA-->>Admin: 200 OK

    Note over Admin,DENA: 2. proba: DENA → Admin
    DENA->>Admin: GET /api/hello
    Admin-->>DENA: 200 OK + agurra
```

| Norabidea | Deskribapena |
|---|---|
| **Administrazioa → DENA** | Zure sistemak DENA-ren endpoint-ak atzitu ditzake (PRE/PRO) |
| **DENA → Administrazioa** | DENA-k zuk eskaintzen dituzun endpoint-ak atzitu ditzake |

---

## Tresna: DENA Admin Connection Test

!!! info ""

    Zure azpiegituran zabaltzen den Spring Boot osagai arina probak exekutatzeko.

    **Biltegia:** [DENA-Euskadi/dena-admin-conx-test]({{ repos.conx_test_tree }})

---

## 1. urratsa: Osagaia zabaldu

!!! warning "Biltegi-bertsioaren egiaztapena"
    
    Klonatu aurretik, egiaztatu biltegi-bertsio zuzena erabiltzen ari zarela. Kontsultatu DENA taldearekin zein den zure lan-ingurunerako gomendatutako bertsioa eta erabili `git checkout <version>` komandoa klonatu ondoren.

=== ":material-application-brackets: Standalone (gomendatua)"

    ```bash
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test
    mvn clean package -Pstandalone
    java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war
    ```

=== ":material-server: WAR lehendik dagoen zerbitzarian"

    ```bash
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test
    mvn clean package
    # WAR-a zure zabalpen-direktoriora kopiatu
    cp denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war /path/to/tomcat/webapps/
    ```

Zerbitzua **8082** portuan abiarazten da.

---

## 2. urratsa: Zure administrazioa konfiguratu

Editatu `denaAdminConxTestRESTApp/src/main/resources/application.yml`:

```yaml
server:
  port: 8082
dena:
  admin: "NombreDeTuAdministracion" # (1)!
  url:
    pre: "https://api-batera.pre.dena.eus/conx-test/api/hello"
    pro: "https://api-batera.pro.dena.eus/conx-test/api/hello"
  conx-test:
    greeting: "Hello %s, welcome to DENA standard!"
    timeout: 10
```

1. Aldatu hau zure administrazioaren benetako izenarekin.

!!! note "URLak zure sarearen arabera"

    | Ingurunea | Internetetik | Euskalsaretik |
    |---|---|---|
    | **PRE** | `https://api-batera.pre.dena.eus/conx-test/api/hello` | `https://api-batera.pre.batera.euskalsarea.eus/conx-test/api/hello` |
    | **PRO** | `https://api-batera.pro.dena.eus/conx-test/api/hello` | `https://api-batera.pro.batera.euskalsarea.eus/conx-test/api/hello` |

---

## 3. urratsa: Administrazioa → DENA balioztatu

```bash
curl -X POST http://localhost:8082/api/conxTest \
  -H "Content-Type: application/json" \
  -d '{"environment": "PRE"}'
```

!!! success "Espero den erantzuna (arrakasta)"

    ```json
    {
      "response": "The request was responded with status 200. Body: ..."
    }
    ```

!!! failure "Huts egiten badu"

    Egiaztatu firewall-a, proxy-a edo DNS-a DENA endpoint-etara. Kontsultatu beheko [Arazoen konponketa](#arazoen-konponketa) atala.

---

## 4. urratsa: DENA → Administrazioa balioztatu

DENA-k zure `/api/hello` endpoint-a deituko du. Egiaztatu eskuragarri dagoela:

```bash
curl http://localhost:8082/api/hello
```

!!! success "Espero den erantzuna"

    ```json
    {
      "invocationResult": "Hello NombreDeTuAdministracion, welcome to DENA standard!"
    }
    ```

!!! warning "DENA saretik irisgarritasuna"

    DENA-k zure sistemara iritsi ahal izateko, portuak DENA saretik irisgarria izan behar du.
    Koordinatu azpiegitura-taldearekin firewall-arauak ireki behar badira.

---

## Balioztapenaren laburpena

| Proba | Komandoa | Espero den emaitza |
|---|---|---|
| :material-check-circle:{ .green } Osagaia martxan | `curl http://localhost:8082/api/hello` | JSON agurrarekin |
| :material-arrow-right: Admin → DENA PRE | `POST /api/conxTest {"environment":"PRE"}` | Status 200 |
| :material-arrow-right: Admin → DENA PRO | `POST /api/conxTest {"environment":"PRO"}` | Status 200 |
| :material-arrow-left: DENA → Admin | DENA-k zure `/api/hello` deitzen du | JSON agurrarekin |

---

## Arazoen konponketa

??? question "Connection refused"

    **Kausa:** Portua itxita edo zerbitzua abiarazi gabe.

    **Konponbidea:** Egiaztatu JAR-a martxan dagoela `ps aux | grep denaAdminConxTest` erabiliz edo abiarazte-logak berrikusiz.

??? question "Connection timeout"

    **Kausa:** Firewall-ak edo proxy-ak konexioa blokeatzen du.

    **Konponbidea:**

    - Egiaztatu firewall-arauak DENA endpoint-etara
    - Probatu `telnet api-batera.pre.dena.eus 443` erabiliz
    - Proxy-a erabiltzen baduzu, konfiguratu `http_proxy` / `https_proxy` aldagaiak

??? question "SSL handshake error"

    **Kausa:** Zerbitzariaren ziurtagiria ez du Java-ren truststore-ak ezagutzen.

    **Konponbidea:**

    ```bash
    # CA ziurtagiria Java-ren truststore-an inportatu
    keytool -import -trustcacerts -alias dena-ca \
      -file dena-ca.crt \
      -keystore $JAVA_HOME/lib/security/cacerts \
      -storepass changeit
    ```

??? question "404 Not Found"

    **Kausa:** Context path okerra.

    **Konponbidea:** Egiaztatu URL osoa. WAR gisa zerbitzari batean zabaldu baduzu, context path-ak artefaktuaren izena izan dezake (adib.: `/denaAdminConxTestRESTApp/api/hello`).

---

## :material-arrow-right-circle: Hurrengo urratsa

<div class="grid cards" markdown>

-   :material-test-tube:{ .lg .middle } **Espedienteen Mock-a**

    ---

    Administrazioaren erantzunak simulatu integrazio-probetarako backend errealik gabe.

    [:octicons-arrow-right-24: Espedienteen Mock-a](./mock-expedientes.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
