# :material-test-tube: Records Mock

Guide to deploy and use the records mock as an integration testing tool with DENA.

---

## What is it for?

!!! abstract "In short"

    The mock simulates an administration that exposes a standard **Data-Retrieve** endpoint.
    It allows you to develop and test without depending on real external services.

- :material-check: Test the full **DENA → Connector → Administration** flow without a real backend
- :material-check: Validate that your integration sends and receives data correctly
- :material-check: Develop against a stable and predictable API

---

## Architecture

``` mermaid
graph LR
    A[DENA CORE] -->|POST /retrieveData| B[Connector Demo1<br/>port 8086]
    B -->|POST /api/retrieveData| C[Records Mock<br/>port 8182]
    C -->|JSON response| B
    B -->|JSON response| A
```

---

## Deployment

### Requirements

| Tool | Version |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-apachemaven: Maven | 3.9+ |
| :material-network: Available port | 8182 (default) |

### Build and start

!!! warning "Repository version selection"
    
    Make sure to clone and select the correct version of the mock repository that is compatible with the version of DENA you are using. Check the versioning documentation before proceeding.

```bash
# Clone the mock repository
git clone <url-repositorio-mock-expedientes>
cd <directorio-mock>

# Build
mvn clean package -Pstandalone

# Start
java -jar target/<nombre-artefacto>.war
```

!!! success "Mock available"

    The service will be listening on `http://localhost:8182`

---

## Configuration

```yaml title="application.yml"
server:
  port: 8182

mock:
  expedientes:
    enabled: true
```

---

## Available endpoints

### Data-Retrieve (DENA standard)

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

## Connection with Connector Demo1

Connector Demo1 (`e80a021h-dena-connector-demo1-rest`) comes preconfigured to connect with the mock:

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

1. The mock does not require OAuth2 authentication.

### Test the full flow

!!! tip "Startup order"

    1. Start the mock (port 8182)
    2. Start Connector Demo1 (port 8086)
    3. Send a request to the connector

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

## Connection with test database (optional)

!!! info "Dynamic data with H2"

    If you need the mock to return dynamic data instead of static responses,
    you can connect it to an embedded H2 database.

```yaml title="application.yml — H2 configuration"
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

Once started, the H2 console will be available at:

:material-open-in-new: `http://localhost:8182/h2-console`

From there you can inspect, insert or modify the test data returned by the mock.

---

## Troubleshooting

??? question "Connection refused to the mock"

    **Check** that the mock is running on the correct port:

    ```bash
    curl http://localhost:8182/api/retrieveData
    # Should respond (even with 405 if you don't send a POST)
    ```

??? question "404 on the endpoint"

    **Check** the context path and the full URL. If you deployed as a WAR, the path may include the artifact name.

??? question "Empty data in the response"

    **Check** that the request parameters (`person.id`, `dataType.id`) match the data loaded in the mock.

??? question "Timeout from the connector to the mock"

    **Increase** `api.timeout` in the connector configuration:

    ```yaml
    dena:
      connector:
        demo1:
          api:
            timeout: 60000  # 60 seconds
    ```

---

## :material-arrow-right-circle: Next step

<div class="grid cards" markdown>

-   :material-code-braces:{ .lg .middle } **Implement your own endpoint**

    ---

    Guide to implement the standard Data-Retrieve endpoint in your real system.

    [:octicons-arrow-right-24: Implementation guide](../semantica/data-retrieve/guia-implementacion.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
