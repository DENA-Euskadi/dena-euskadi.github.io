# :material-connection: Testing communications with DENA

Step-by-step guide to validate bidirectional connectivity between your administration and DENA.

---

## What is validated?

``` mermaid
sequenceDiagram
    participant Admin as Your Administration
    participant DENA as DENA PRE/PRO

    Note over Admin,DENA: Test 1: Admin → DENA
    Admin->>DENA: POST /api/conxTest
    DENA-->>Admin: 200 OK

    Note over Admin,DENA: Test 2: DENA → Admin
    DENA->>Admin: GET /api/hello
    Admin-->>DENA: 200 OK + greeting
```

| Direction | Description |
|---|---|
| **Administration → DENA** | Your system can reach DENA endpoints (PRE/PRO) |
| **DENA → Administration** | DENA can reach the endpoints you expose |

---

## Tool: DENA Admin Connection Test

!!! info ""

    Lightweight Spring Boot component that is deployed in your infrastructure to run the tests.

    **Repository:** [DENA-Euskadi/dena-admin-conx-test]({{ repos.conx_test_tree }})

---

## Step 1: Deploy the component

!!! warning "Repository version verification"
    
    Before proceeding with cloning, verify that you are using the correct version of the repository. Check with the DENA team which is the recommended version for your working environment and use the `git checkout <version>` command after cloning.

=== ":material-application-brackets: Standalone (recommended)"

    ```bash
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test
    mvn clean package -Pstandalone
    java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war
    ```

=== ":material-server: WAR on existing server"

    ```bash
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test
    mvn clean package
    # Copy the WAR to your deployment directory
    cp denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war /path/to/tomcat/webapps/
    ```

The service starts on port **8082**.

---

## Step 2: Configure your administration

Edit `denaAdminConxTestRESTApp/src/main/resources/application.yml`:

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

1. Change this to the real name of your administration.

!!! note "URLs depending on your network"

    | Environment | From Internet | From Euskalsarea |
    |---|---|---|
    | **PRE** | `https://api-batera.pre.dena.eus/conx-test/api/hello` | `https://api-batera.pre.batera.euskalsarea.eus/conx-test/api/hello` |
    | **PRO** | `https://api-batera.pro.dena.eus/conx-test/api/hello` | `https://api-batera.pro.batera.euskalsarea.eus/conx-test/api/hello` |

---

## Step 3: Validate Administration → DENA

```bash
curl -X POST http://localhost:8082/api/conxTest \
  -H "Content-Type: application/json" \
  -d '{"environment": "PRE"}'
```

!!! success "Expected response (success)"

    ```json
    {
      "response": "The request was responded with status 200. Body: ..."
    }
    ```

!!! failure "If it fails"

    Check firewall, proxy or DNS towards DENA endpoints. See the [Troubleshooting](#troubleshooting) section below.

---

## Step 4: Validate DENA → Administration

DENA will invoke your `/api/hello` endpoint. Verify it is accessible:

```bash
curl http://localhost:8082/api/hello
```

!!! success "Expected response"

    ```json
    {
      "invocationResult": "Hello NombreDeTuAdministracion, welcome to DENA standard!"
    }
    ```

!!! warning "Accessibility from the DENA network"

    For DENA to reach your system, the port must be accessible from the DENA network.
    Coordinate with the infrastructure team if firewall rules need to be opened.

---

## Validation summary

| Test | Command | Expected result |
|---|---|---|
| :material-check-circle:{ .green } Component running | `curl http://localhost:8082/api/hello` | JSON with greeting |
| :material-arrow-right: Admin → DENA PRE | `POST /api/conxTest {"environment":"PRE"}` | Status 200 |
| :material-arrow-right: Admin → DENA PRO | `POST /api/conxTest {"environment":"PRO"}` | Status 200 |
| :material-arrow-left: DENA → Admin | DENA invokes your `/api/hello` | JSON with greeting |

---

## Troubleshooting

??? question "Connection refused"

    **Cause:** Port closed or service not started.

    **Solution:** Verify that the JAR is running with `ps aux | grep denaAdminConxTest` or check the startup logs.

??? question "Connection timeout"

    **Cause:** Firewall or proxy blocking the connection.

    **Solution:**

    - Check firewall rules towards DENA endpoints
    - Test with `telnet api-batera.pre.dena.eus 443`
    - If using a proxy, configure the `http_proxy` / `https_proxy` variables

??? question "SSL handshake error"

    **Cause:** Server certificate not recognized by the Java truststore.

    **Solution:**

    ```bash
    # Import the CA certificate into the Java truststore
    keytool -import -trustcacerts -alias dena-ca \
      -file dena-ca.crt \
      -keystore $JAVA_HOME/lib/security/cacerts \
      -storepass changeit
    ```

??? question "404 Not Found"

    **Cause:** Incorrect context path.

    **Solution:** Verify the full URL. If you deployed as a WAR on a server, the context path may include the artifact name (e.g.: `/denaAdminConxTestRESTApp/api/hello`).

---

## :material-arrow-right-circle: Next step

<div class="grid cards" markdown>

-   :material-test-tube:{ .lg .middle } **Records Mock**

    ---

    Simulate administration responses for integration testing without a real backend.

    [:octicons-arrow-right-24: Records Mock](./mock-expedientes.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
