# :material-test-tube: Espedienteen Mock-a

Gida DENA-rekin integrazio-probak egiteko espedienteen mock-a zabaldu eta erabiltzeko.

---

## Zertarako da?

!!! abstract "Laburbilduz"

    Mock-ak **Data-Retrieve** endpoint estandarra eskaintzen duen administrazio bat simulatzen du.
    Kanpoko benetako zerbitzuetan dependentziarik gabe garatu eta probatzeko aukera ematen du.

- :material-check: **DENA → Konektorea → Administrazioa** fluxu osoa probatu backend errealik gabe
- :material-check: Zure integrazioak datuak behar bezala bidaltzen eta jasotzen dituela egiaztatu
- :material-check: API egonkor eta aurreikusgarri baten aurka garatu

---

## Arkitektura

``` mermaid
graph LR
    A[DENA CORE] -->|POST /retrieveData| B[Demo1 Konektorea<br/>8086 portua]
    B -->|POST /api/retrieveData| C[Espedienteen Mock-a<br/>8182 portua]
    C -->|JSON erantzuna| B
    B -->|JSON erantzuna| A
```

---

## Zabalpena

### Eskakizunak

| Tresna | Bertsioa |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-apachemaven: Maven | 3.9+ |
| :material-network: Portu erabilgarria | 8182 (lehenetsita) |

### Konpilatu eta abiarazi

!!! warning "Biltegi-bertsioaren hautaketa"
    
    Ziurtatu erabiltzen ari zaren DENA bertsioarekin bateragarria den mock-aren biltegi-bertsio zuzena klonatzen eta hautatzen duzula. Kontsultatu bertsio-dokumentazioa aurrera egin aurretik.

```bash
# Mock-aren biltegia klonatu
git clone <url-repositorio-mock-expedientes>
cd <directorio-mock>

# Konpilatu
mvn clean package -Pstandalone

# Abiarazi
java -jar target/<nombre-artefacto>.war
```

!!! success "Mock-a erabilgarri"

    Zerbitzua `http://localhost:8182` helbidean entzuten egongo da

---

## Konfigurazioa

```yaml title="application.yml"
server:
  port: 8182

mock:
  expedientes:
    enabled: true
```

---

## Endpoint erabilgarriak

### Data-Retrieve (DENA estandarra)

!!! example "POST /api/retrieveData"

    === "Eskaera"

        ```json
        {
          "dataType": { "id": "RECORD", "oid": "..." },
          "admin": { "id": "demo_admin1", "oid": "..." },
          "person": { "id": "12345678A", "oid": "..." },
          "firstRowNum": 0,
          "numberOfRows": 10
        }
        ```

    === "Erantzuna (200 OK)"

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

## Demo1 Konektorearekin konexioa

Demo1 konektorea (`e80a021h-dena-connector-demo1-rest`) mock-arekin konektatzeko aurrekonfiguratuta dator:

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

1. Mock-ak ez du OAuth2 autentifikaziorik behar.

### Fluxu osoa probatu

!!! tip "Abiarazte-ordena"

    1. Mock-a abiarazi (8182 portua)
    2. Demo1 Konektorea abiarazi (8086 portua)
    3. Eskaera bat bidali konektoreari

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

## Proba-datu-basearekin konexioa (aukerakoa)

!!! info "Datu dinamikoak H2-rekin"

    Mock-ak erantzun estatikoen ordez datu dinamikoak itzultzea behar baduzu,
    H2 datu-base txertatuarekin konekta dezakezu.

```yaml title="application.yml — H2 konfigurazioa"
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

Abiarazita dagoenean, H2 kontsola hemen egongo da erabilgarri:

:material-open-in-new: `http://localhost:8182/h2-console`

Bertatik mock-ak itzultzen dituen proba-datuak ikuskatu, txertatu edo aldatu ditzakezu.

---

## Arazoen konponketa

??? question "Connection refused mock-era"

    **Egiaztatu** mock-a portu egokian abiarazita dagoela:

    ```bash
    curl http://localhost:8182/api/retrieveData
    # Erantzun beharko luke (nahiz eta 405 itzuli POST bidaltzen ez baduzu)
    ```

??? question "404 endpoint-ean"

    **Egiaztatu** context path-a eta URL osoa. WAR gisa zabaldu baduzu, baliteke bide-izenak artefaktuaren izena izatea.

??? question "Datu hutsak erantzunean"

    **Egiaztatu** eskaeraren parametroak (`person.id`, `dataType.id`) mock-ak kargatuta dituen datuekin bat datozela.

??? question "Timeout konektoretik mock-era"

    **Handitu** `api.timeout` konektorearen konfigurazioan:

    ```yaml
    dena:
      connector:
        demo1:
          api:
            timeout: 60000  # 60 segundo
    ```

---

## :material-arrow-right-circle: Hurrengo urratsa

<div class="grid cards" markdown>

-   :material-code-braces:{ .lg .middle } **Zure endpoint propioa inplementatu**

    ---

    Zure sistema errealean Data-Retrieve endpoint estandarra inplementatzeko gida.

    [:octicons-arrow-right-24: Inplementazio-gida](../semantica/data-retrieve/guia-implementacion.md)

</div>

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
