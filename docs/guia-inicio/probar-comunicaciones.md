# :material-connection: Probar las comunicaciones con DENA

Guía paso a paso para validar la conectividad bidireccional entre tu administración y DENA.

---

## ¿Qué se valida?

``` mermaid
sequenceDiagram
    participant Admin as Tu Administración
    participant DENA as DENA PRE/PRO

    Note over Admin,DENA: Test 1: Admin → DENA
    Admin->>DENA: POST /api/conxTest
    DENA-->>Admin: 200 OK

    Note over Admin,DENA: Test 2: DENA → Admin
    DENA->>Admin: GET /api/hello
    Admin-->>DENA: 200 OK + saludo
```

| Dirección | Descripción |
|---|---|
| **Administración → DENA** | Tu sistema puede alcanzar los endpoints de DENA (PRE/PRO) |
| **DENA → Administración** | DENA puede alcanzar los endpoints que tú expones |

---

## Herramienta: DENA Admin Connection Test

!!! info ""

    Componente Spring Boot ligero que se despliega en tu infraestructura para ejecutar las pruebas.

    **Repositorio:** [DENA-Euskadi/dena-admin-conx-test]({{ repos.conx_test_tree }})

---

## Paso 1: Desplegar el componente

=== ":material-application-brackets: Standalone (recomendado)"

    ```bash
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test
    mvn clean package -Pstandalone
    java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war
    ```

=== ":material-server: WAR en servidor existente"

    ```bash
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test
    mvn clean package
    # Copiar el WAR a tu directorio de despliegue
    cp denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war /path/to/tomcat/webapps/
    ```

El servicio arranca en el puerto **8082**.

---

## Paso 2: Configurar tu administración

Edita `denaAdminConxTestRESTApp/src/main/resources/application.yml`:

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

1. Cambia esto por el nombre real de tu administración.

!!! note "URLs según tu red"

    | Entorno | Desde Internet | Desde Euskalsarea |
    |---|---|---|
    | **PRE** | `https://api-batera.pre.dena.eus/conx-test/api/hello` | `https://api-batera.pre.batera.euskalsarea.eus/conx-test/api/hello` |
    | **PRO** | `https://api-batera.pro.dena.eus/conx-test/api/hello` | `https://api-batera.pro.batera.euskalsarea.eus/conx-test/api/hello` |

---

## Paso 3: Validar Administración → DENA

```bash
curl -X POST http://localhost:8082/api/conxTest \
  -H "Content-Type: application/json" \
  -d '{"environment": "PRE"}'
```

!!! success "Respuesta esperada (éxito)"

    ```json
    {
      "response": "The request was responded with status 200. Body: ..."
    }
    ```

!!! failure "Si falla"

    Revisa firewall, proxy o DNS hacia los endpoints DENA. Consulta la sección de [Troubleshooting](#troubleshooting) más abajo.

---

## Paso 4: Validar DENA → Administración

DENA invocará tu endpoint `/api/hello`. Verifica que está accesible:

```bash
curl http://localhost:8082/api/hello
```

!!! success "Respuesta esperada"

    ```json
    {
      "invocationResult": "Hello NombreDeTuAdministracion, welcome to DENA standard!"
    }
    ```

!!! warning "Accesibilidad desde la red DENA"

    Para que DENA pueda llegar a tu sistema, el puerto debe ser accesible desde la red DENA.
    Coordina con el equipo de infraestructura si es necesario abrir reglas de firewall.

---

## Resumen de validación

| Test | Comando | Resultado esperado |
|---|---|---|
| :material-check-circle:{ .green } Componente levantado | `curl http://localhost:8082/api/hello` | JSON con saludo |
| :material-arrow-right: Admin → DENA PRE | `POST /api/conxTest {"environment":"PRE"}` | Status 200 |
| :material-arrow-right: Admin → DENA PRO | `POST /api/conxTest {"environment":"PRO"}` | Status 200 |
| :material-arrow-left: DENA → Admin | DENA invoca tu `/api/hello` | JSON con saludo |

---

## Troubleshooting

??? question "Connection refused"

    **Causa:** Puerto cerrado o servicio no arrancado.

    **Solución:** Verificar que el JAR está corriendo con `ps aux | grep denaAdminConxTest` o revisar los logs de arranque.

??? question "Connection timeout"

    **Causa:** Firewall o proxy bloqueando la conexión.

    **Solución:**

    - Verificar reglas de firewall hacia los endpoints DENA
    - Probar con `telnet api-batera.pre.dena.eus 443`
    - Si usas proxy, configurar las variables `http_proxy` / `https_proxy`

??? question "SSL handshake error"

    **Causa:** Certificado del servidor no reconocido por el truststore de Java.

    **Solución:**

    ```bash
    # Importar el certificado CA al truststore de Java
    keytool -import -trustcacerts -alias dena-ca \
      -file dena-ca.crt \
      -keystore $JAVA_HOME/lib/security/cacerts \
      -storepass changeit
    ```

??? question "404 Not Found"

    **Causa:** Context path incorrecto.

    **Solución:** Verificar la URL completa. Si desplegaste como WAR en un servidor, el context path puede incluir el nombre del artefacto (ej: `/denaAdminConxTestRESTApp/api/hello`).

---

## :material-arrow-right-circle: Siguiente paso

<div class="grid cards" markdown>

-   :material-test-tube:{ .lg .middle } **Mock de Expedientes**

    ---

    Simular respuestas de administración para pruebas de integración sin backend real.

    [:octicons-arrow-right-24: Mock de Expedientes](./mock-expedientes.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
