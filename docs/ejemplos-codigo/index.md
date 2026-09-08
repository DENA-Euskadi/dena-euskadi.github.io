# :material-file-code: Ejemplos de Codigo

Implementaciones de referencia para cada operativa DENA. Codigo funcional que puedes copiar y adaptar.

---

## Data-Retrieve (Spring Boot)

Endpoint minimo que recibe una peticion de DENA y devuelve expedientes:

### Controller

```java
@RestController
@RequestMapping("/api")
@RequiredArgsConstructor
public class DenaRetrieveDataController {

    private final ExpedienteRepository repository;

    @PostMapping(value = "/retrieveData",
                 consumes = MediaType.APPLICATION_JSON_VALUE,
                 produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<Map<String, Object>> retrieveData(
            @RequestBody Map<String, Object> request) {

        // 1. Extraer personId y dataTypeId del context
        Map<String, Object> context = (Map<String, Object>) request.get("context");
        Map<String, String> subjectPerson = (Map<String, String>) context.get("subjectPerson");
        Map<String, String> dataType = (Map<String, String>) context.get("dataType");

        String personId = subjectPerson.get("personId");
        String dataTypeId = dataType.get("dataTypeId");

        // 2. Consultar datos segun tipo
        List<Map<String, Object>> dataItems = switch (dataTypeId) {
            case "RECORDS"  -> fetchRecords(personId);
            case "NOTICES"  -> fetchNotices(personId);
            case "REGISTER" -> fetchRegistry(personId);
            case "PAYMENTS" -> fetchPayments(personId);
            case "SCHEDULE" -> fetchSchedule(personId);
            default         -> List.of();
        };

        // 3. Construir response
        Map<String, Object> response = buildResponse(context, dataItems);
        return ResponseEntity.ok(response);
    }

    private List<Map<String, Object>> fetchRecords(String personId) {
        return repository.findByPersonId(personId).stream()
            .map(this::toRecordItem)
            .toList();
    }

    private Map<String, Object> toRecordItem(Expediente exp) {
        return Map.ofEntries(
            Map.entry("oid", exp.getId().toString()),
            Map.entry("id", exp.getNumero()),
            Map.entry("lastChangedAt", exp.getUpdatedAt().toString()),
            Map.entry("service", Map.of(
                "serviceNameByLanguage", Map.of(
                    "SPANISH", exp.getServicioEs(),
                    "BASQUE", exp.getServicioEu()
                ),
                "originRef", Map.of("id", exp.getServicioId())
            )),
            Map.entry("procedure", Map.of(
                "serviceNameByLanguage", Map.of(
                    "SPANISH", exp.getProcedimientoEs(),
                    "BASQUE", exp.getProcedimientoEu()
                ),
                "originRef", Map.of("id", exp.getProcedimientoId())
            )),
            Map.entry("createdAt", exp.getCreatedAt().toString()),
            Map.entry("state", Map.of(
                "stateCode", exp.getEstado().name(),
                "description", Map.of(
                    "SPANISH", exp.getEstadoDescEs(),
                    "BASQUE", exp.getEstadoDescEu()
                )
            )),
            Map.entry("urls", List.of(
                Map.of("url", "https://sede.tuadmin.eus/exp/" + exp.getNumero(),
                       "language", "SPANISH", "tags", List.of("default")),
                Map.of("url", "https://egoitza.tuadmin.eus/esp/" + exp.getNumero(),
                       "language", "BASQUE", "tags", List.of("default"))
            ))
        );
    }

    private Map<String, Object> buildResponse(Map<String, Object> requestContext,
                                               List<Map<String, Object>> dataItems) {
        // Copiar context cambiando flowDirection a RESPONSE
        Map<String, Object> responseContext = new HashMap<>(requestContext);
        responseContext.put("flowDirection", "RESPONSE");

        return Map.of(
            "context", responseContext,
            "data", Map.of("dataItems", dataItems),
            "code", "OK"
        );
    }

    // Implementa fetchNotices, fetchRegistry, fetchPayments, fetchSchedule
    // siguiendo el mismo patron...
    private List<Map<String, Object>> fetchNotices(String personId) { return List.of(); }
    private List<Map<String, Object>> fetchRegistry(String personId) { return List.of(); }
    private List<Map<String, Object>> fetchPayments(String personId) { return List.of(); }
    private List<Map<String, Object>> fetchSchedule(String personId) { return List.of(); }
}
```

### Entity

```java
@Entity
@Table(name = "expedientes")
@Data
public class Expediente {
    @Id
    private UUID id;
    private String numero;
    private String personId;
    private String servicioEs;
    private String servicioEu;
    private String servicioId;
    private String procedimientoEs;
    private String procedimientoEu;
    private String procedimientoId;
    private Instant createdAt;
    private Instant updatedAt;
    @Enumerated(EnumType.STRING)
    private EstadoExpediente estado;
    private String estadoDescEs;
    private String estadoDescEu;
}

public enum EstadoExpediente {
    REGISTERED_PENDING_TO_BE_OPENED,
    OPENED,
    IN_PROGRESS,
    WAITING_FOR_INTERESTED_PARTY_RESPONSE,
    WAITING_FOR_OTHER_ORG_WORK,
    CLOSED
}
```

### Repository

```java
public interface ExpedienteRepository extends JpaRepository<Expediente, UUID> {
    List<Expediente> findByPersonId(String personId);
}
```

---

## Metadata-Sync (enviar SRMD)

Servicio que consulta cambios en tu BD y los envia a DENA periodicamente:

```java
@Service
@RequiredArgsConstructor
@Slf4j
public class MetadataSyncService {

    private final JdbcTemplate jdbc;
    private final DenaTokenService tokenService;
    private final RestClient restClient;

    @Value("${dena.srmd.url}")
    private String srmdUrl;

    @Value("${dena.admin.id}")
    private String adminId;

    /**
     * Ejecutar cada X minutos via @Scheduled o cron
     */
    @Scheduled(fixedRate = 300_000) // cada 5 minutos
    public void syncMetadata() {
        Instant since = getLastSyncTimestamp();
        List<PersonChange> changes = findChangesSince(since);

        if (changes.isEmpty()) {
            log.info("No hay cambios desde {}", since);
            return;
        }

        // Convertir a formato SRMD
        List<Map<String, Object>> srmdItems = changes.stream()
            .map(change -> Map.<String, Object>of(
                "admin", Map.of("id", adminId),
                "aboutPerson", Map.of("id", change.personId()),
                "someDataWasUpdatedAt", change.lastChangedAt().toString(),
                "ofType", Map.of("id", change.dataTypeId()),
                "fromDataOrigin", "DEFAULT"
            ))
            .toList();

        // Enviar a DENA
        String token = tokenService.getToken();

        String response = restClient.post()
            .uri(srmdUrl)
            .header("Authorization", "Bearer " + token)
            .contentType(MediaType.APPLICATION_JSON)
            .body(srmdItems)
            .retrieve()
            .body(String.class);

        log.info("SRMD enviado: {} items. Respuesta: {}", srmdItems.size(), response);
        saveLastSyncTimestamp(Instant.now());
    }

    private List<PersonChange> findChangesSince(Instant since) {
        return jdbc.query("""
            SELECT PERSON_ID, 'RECORDS' AS DATA_TYPE_ID,
                   MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE
              FROM EXPEDIENTES
             WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= ?
             GROUP BY PERSON_ID
            """,
            (rs, rowNum) -> new PersonChange(
                rs.getString("PERSON_ID"),
                rs.getString("DATA_TYPE_ID"),
                rs.getTimestamp("LAST_CHANGE").toInstant()
            ),
            Timestamp.from(since)
        );
    }

    private Instant getLastSyncTimestamp() { /* leer de tabla de control */ }
    private void saveLastSyncTimestamp(Instant ts) { /* guardar en tabla de control */ }
}

record PersonChange(String personId, String dataTypeId, Instant lastChangedAt) {}
```

---

## Obtencion de Token (servicio reutilizable)

```java
@Service
@Slf4j
public class DenaTokenService {

    @Value("${dena.auth.token-url}")
    private String tokenUrl;

    @Value("${dena.auth.client-id}")
    private String clientId;

    @Value("${dena.auth.client-secret}")
    private String clientSecret;

    private String cachedToken;
    private Instant tokenExpiry = Instant.MIN;

    private static final int LEEWAY_SECONDS = 60;

    public synchronized String getToken() {
        if (cachedToken != null && Instant.now().isBefore(tokenExpiry)) {
            return cachedToken;
        }
        return refreshToken();
    }

    private String refreshToken() {
        RestClient client = RestClient.create();

        Map<String, Object> response = client.post()
            .uri(tokenUrl)
            .contentType(MediaType.APPLICATION_FORM_URLENCODED)
            .body("grant_type=client_credentials"
                + "&client_id=" + clientId
                + "&client_secret=" + clientSecret)
            .retrieve()
            .body(new ParameterizedTypeReference<>() {});

        cachedToken = (String) response.get("access_token");
        int expiresIn = (int) response.get("expires_in");
        tokenExpiry = Instant.now().plusSeconds(expiresIn - LEEWAY_SECONDS);

        log.debug("Token renovado, expira en {}s", expiresIn);
        return cachedToken;
    }
}
```

---

## application.yml

```yaml
dena:
  admin:
    id: "TU-ADMIN-ID"
  auth:
    token-url: "https://api-batera.pre.dena.eus/auth/realms/DenaAuthAdmins/protocol/openid-connect/token"
    client-id: "${DENA_CLIENT_ID}"
    client-secret: "${DENA_CLIENT_SECRET}"
  srmd:
    url: "https://api-batera.pre.dena.eus/srmd/"
```

---

## Dependencias Maven

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

!!! tip "Dependencias DENA opcionales"
    Si quieres usar los model objects de DENA directamente (en lugar de `Map<String, Object>`), anade:

    ```xml
    <dependency>
        <groupId>dena.api.common</groupId>
        <artifactId>denaCommonAPIModelClasses</artifactId>
        <version>{{ dena.version }}</version>
    </dependency>
    ```

    Esto requiere acceso a los repositorios Maven de DENA. Ver [:octicons-arrow-right-24: Instalacion](../guia-inicio/instalacion.md).

---

## Otros lenguajes

Para snippets en C#, Python, Node.js y PHP del endpoint Data-Retrieve:

[:octicons-arrow-right-24: Snippets de codigo](../semantica/data-retrieve/snippets-codigo.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
