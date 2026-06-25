# :material-test-tube: Mock de Expedientes

Guía para desplegar y utilizar el mock de expedientes como herramienta de pruebas de integración con DENA.

---

## ¿Para qué sirve?

!!! abstract "En resumen"

    El mock simula una administración que expone un endpoint **Data-Retrieve** estándar.
    Permite desarrollar y probar sin depender de servicios externos reales.

- :material-check: Probar el flujo completo **DENA → Conector → Administración** sin un backend real
- :material-check: Validar que tu integración envía y recibe correctamente los datos
- :material-check: Desarrollar contra una API estable y predecible

---

## Arquitectura

``` mermaid
graph LR
    A[DENA CORE] -->|POST /retrieveData| B[Conector Demo1<br/>puerto 8086]
    B -->|POST /api/retrieveData| C[Mock Expedientes<br/>puerto 8182]
    C -->|JSON response| B
    B -->|JSON response| A
```

---

## Despliegue

### Requisitos

| Herramienta | Versión |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-apachemaven: Maven | 3.9+ |
| :material-network: Puerto disponible | 8182 (por defecto) |

### Compilar y arrancar

```bash
# Clonar el repositorio del mock
git clone <url-repositorio-mock-expedientes>
cd <directorio-mock>

# Compilar
mvn clean package -Pstandalone

# Arrancar
java -jar target/<nombre-artefacto>.war
```

!!! success "Mock disponible"

    El servicio estará escuchando en `http://localhost:8182`

---

## Configuración

```yaml title="application.yml"
server:
  port: 8182

mock:
  expedientes:
    enabled: true
```

---

## Endpoints disponibles

### Data-Retrieve (estándar DENA)

!!! example "POST /api/retrieveData"

    === "Request"

        ```json
        {
          "dataType": { "id": "RECORD", "oid": "..." },
          "admin": { "id": "demo_admin1", "oid": "..." },
          "person": { "id": "12345678A", "oid": "..." },
          "firstRowNum": 0,
          "numberOfRows": 10
        }
        ```

    === "Response (200 OK)"

        ```json
        {
          "data": [
            {
              "id": "EXP-001",
              "title": "Expediente de ejemplo",
              "status": "EN_TRAMITE",
              "startDate": "2025-01-15",
              "lastUpdate": "2026-06-01"
            }
          ],
          "totalCount": 1
        }
        ```

---

## Conexión con el Conector Demo1

El conector Demo1 (`e80a021h-dena-connector-demo1-rest`) viene preconfigurado para conectar con el mock:

```yaml title="application.yml del conector Demo1"
dena:
  connector:
    demo1:
      api:
        url: ${ADMIN_API_URL:http://localhost:8182}
        timeout: 30000
      oauth:
        enabled: false  # (1)!
```

1. El mock no requiere autenticación OAuth2.

### Probar el flujo completo

!!! tip "Orden de arranque"

    1. Arrancar el mock (puerto 8182)
    2. Arrancar el conector Demo1 (puerto 8086)
    3. Enviar request al conector

```bash
curl -X POST http://localhost:8086/api/connector/retrieveData \
  -H "Content-Type: application/json" \
  -d '{
    "data": {
      "dataOriginConfigForDataTypeInAdmin": {
        "remotePartyConnectorConfig": {
          "transportConfig": {
            "destinationUrl": "http://localhost:8182/api/retrieveData"
          }
        }
      },
      "dataRetrieveRequest": {
        "dataType": { "id": "RECORD" },
        "admin": { "id": "demo_admin1" },
        "person": { "id": "12345678A" },
        "firstRowNum": 0,
        "numberOfRows": 10
      }
    }
  }'
```

---

## Conexión con base de datos de pruebas (opcional)

!!! info "Datos dinámicos con H2"

    Si necesitas que el mock devuelva datos dinámicos en lugar de respuestas estáticas,
    puedes conectarlo a una base de datos embebida H2.

```yaml title="application.yml — Configuración H2"
spring:
  datasource:
    url: jdbc:h2:mem:mockdb
    driver-class-name: org.h2.Driver
    username: sa
    password:
  h2:
    console:
      enabled: true
      path: /h2-console
```

Una vez arrancado, la consola H2 estará disponible en:

:material-open-in-new: `http://localhost:8182/h2-console`

Desde ahí puedes inspeccionar, insertar o modificar los datos de prueba que devuelve el mock.

---

## Troubleshooting

??? question "Connection refused al mock"

    **Verificar** que el mock está arrancado en el puerto correcto:

    ```bash
    curl http://localhost:8182/api/retrieveData
    # Debería responder (aunque sea con 405 si no envías POST)
    ```

??? question "404 en el endpoint"

    **Verificar** el context path y la URL completa. Si desplegaste como WAR, puede que el path incluya el nombre del artefacto.

??? question "Datos vacíos en la respuesta"

    **Verificar** que los parámetros del request (`person.id`, `dataType.id`) coinciden con los datos que tiene cargado el mock.

??? question "Timeout del conector hacia el mock"

    **Aumentar** `api.timeout` en la configuración del conector:

    ```yaml
    dena:
      connector:
        demo1:
          api:
            timeout: 60000  # 60 segundos
    ```

---

## :material-arrow-right-circle: Siguiente paso

<div class="grid cards" markdown>

-   :material-code-braces:{ .lg .middle } **Implementar tu propio endpoint**

    ---

    Guía para implementar el endpoint Data-Retrieve estándar en tu sistema real.

    [:octicons-arrow-right-24: Guía de implementación](../semantica/data-retrieve/guia-implementacion.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
