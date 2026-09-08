# :material-database-arrow-right: Data-Retrieve (Servir Datos)

## Concepto

Data-Retrieve es la operativa por la cual **DENA llama a tu administración para obtener los datos de una persona**. Tu admin expone un endpoint REST y DENA lo invoca cuando la persona necesita ver sus datos actualizados.

![Data Retrieval Proxy](../adjuntos/imagenes/arquitectura/services-data-retrieval-proxy.png)

### ¿Cómo funciona?

1. La persona abre la app y DENA detecta que hay datos pendientes de refrescar
2. DENA-CORE invoca tu endpoint a través del conector de tu admin
3. Tu endpoint consulta la BD y devuelve los datos de esa persona
4. DENA-CORE determina qué datos son nuevos/actualizados y los envía a la app

![Data Retrieve Steps](../adjuntos/imagenes/arquitectura/services-data-retrieve-steps.png)

### Puntos clave

- Tu admin devuelve **todos los datos** del tipo de dato solicitado para esa persona
- Cada data item debe incluir un campo **`lastChangedAt`** con el instante del último cambio
- DENA-CORE calcula automáticamente si el dato es nuevo/actualizado (la admin NO necesita indicarlo)
- Los datos **no se almacenan en DENA** — se pasan directamente a la app

---

## Implementación

### Endpoint

Tu admin expone un endpoint que DENA llamará:

```
POST /api/retrieveData
```

El endpoint recibe la persona y el tipo de dato solicitado, y devuelve los data items.

### Ejemplo: tabla de base de datos

Supongamos un tipo de dato "citas de atención ciudadana" con esta estructura:

| PERSON_ID | AT | DURATION_MIN | SUBJECT_ES | SUBJECT_EU | LAST_UPDATED_AT |
|-----------|-----|------|------------|------------|-----------------|
| 48291038Z | 2026-08-17T14:30 | 30 | Cita renovación | Berritze hitzordua | 2026-08-17T09:14:22Z |
| 48291038Z | 2026-09-04T10:15 | 15 | Consulta estado | Egoera kontsulta | 2026-08-17T18:41:03Z |

### Ejemplo: consulta SQL

```sql
SELECT PERSON_ID, AT, DURATION_MIN, SUBJECT_ES, SUBJECT_EU,
       COALESCE(LAST_UPDATED_AT, CREATED_AT) AS LAST_CHANGE_AT
  FROM APPOINTMENTS
 WHERE PERSON_ID = ?
```

### Ejemplo: conversión a modelo DENA

```java
private List<DN00ScheduleItem> _fromDBDataToReturnedDataItems(
        final Collection<DBData> dbData) {
    return dbData.stream()
        .map(item -> {
            DN00ScheduleItem schItem = new DN00ScheduleItem();

            // IMPORTANTE: instante de último cambio
            schItem.setLastChangedAt(item.lastChangedAt());
            schItem.setId(DN00ScheduleItemID.forId(item.id()));
            schItem.setAt(item.at());
            schItem.setDurationMinutes(item.durationMins());

            // Textos multiidioma
            schItem.setSubjectByLanguage(new LanguageTextsMapBacked()
                .add(Language.SPANISH, item.subjectES())
                .add(Language.BASQUE, item.subjectEU()));

            // URLs de detalle en tu web
            schItem.setUrls(UrlCollection.create()
                .addItems(
                    UrlCollectionItem.createFor(
                        Url.from("https://my-admin.eus/citas/" + item.id() + "?lang=eu")),
                        CollectionItemID.forId("main"), Language.BASQUE),
                    UrlCollectionItem.createFor(
                        Url.from("https://my-admin.eus/citas/" + item.id() + "?lang=es")),
                        CollectionItemID.forId("main"), Language.SPANISH)));

            return schItem;
        })
        .toList();
}
```

### Ejemplo: REST Controller

```java
@RestController
@RequestMapping("/dena/retrieve")
public class AdminAppointmentsController {

    @GetMapping("/citizen_care_appointments/{personId}")
    public ResponseEntity<String> getByPerson(
            @PathVariable("personId") String personIdParam) {

        DN00PersonID personId = DN00PersonID.forId(personIdParam);

        // 1. Consultar BD
        List<DBData> dbData = _retrieveDBDataFor(personId);

        // 2. Convertir a modelo DENA
        List<DN00ScheduleItem> items = _fromDBDataToReturnedDataItems(dbData);

        // 3. Serializar y devolver
        String json = _marshaller.forWriting().toJSON(items);
        return ResponseEntity.ok(json);
    }
}
```

### Ejemplo: JSON devuelto

La respuesta completa incluye la estructura del mensaje interop DENA (context + data). Los data items van dentro de `data.dataItems`:

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "dataType": { "dataTypeId": "SCHEDULE" },
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "48291038Z" }
  },
  "data": {
    "dataItems": [
      {
        "id": "appointment123",
        "lastChangedAt": "2026-08-19T08:07:56.742Z",
        "year": "2026",
        "monthOfYear": "8",
        "dayOfMonth": "18",
        "hourOfDay": "13",
        "minuteOfHour": "2",
        "durationMinutes": 30,
        "subject": {
          "SPANISH": "una cita",
          "BASQUE": "hitzordu bat"
        },
        "urls": [
          { "id": "main", "lang": "BASQUE", "value": "https://my-admin.eus/citas/appointment123?lang=eu" },
          { "id": "main", "lang": "SPANISH", "value": "https://my-admin.eus/citas/appointment123?lang=es" }
        ]
      }
    ]
  },
  "code": "OK"
}
```

??? note "Estructura completa del mensaje (con todos los campos de semantica base)"

    En una respuesta real, el mensaje puede incluir campos adicionales de protocolo, consentimiento y trazabilidad que DENA gestiona automaticamente. Tu administracion solo necesita preocuparse del bloque `data.dataItems`, pero a continuacion se muestra la estructura completa para referencia:

    ```json
    {
      "context": {
        "messageType": "PERSON_FETCH_DATA",
        "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
        "flowDirection": "RESPONSE",
        "originPartyId": "MI-ADMIN",
        "destinationPartyId": "DENA-CORE",
        "userAgent": "MiAdmin/1.0 data-provider/1.0 (soporte@miadmin.eus)",
        "dataType": { "dataTypeId": "SCHEDULE" },
        "subjectPerson": { "personId": "48291038Z" },
        "administration": { "administrationId": "MI-ADMIN", "dir3Code": "EA0000001" },
        "interopRouteData": [
          { "denaComponentId": "DENA_ADMIN_CONNECTOR", "timestamp": "2026-08-19T08:07:55.100Z" },
          { "denaComponentId": "ADMIN", "timestamp": "2026-08-19T08:07:56.742Z" }
        ]
      },
      "protocol": {
        "urls": [],
        "timeOut": "30s"
      },
      "consent": {
        "consentOid": "CONSENT-OID-2026-001"
      },
      "data": {
        "dataItems": [
          {
            "id": "appointment123",
            "lastChangedAt": "2026-08-19T08:07:56.742Z",
            "year": "2026",
            "monthOfYear": "8",
            "dayOfMonth": "18",
            "hourOfDay": "13",
            "minuteOfHour": "2",
            "durationMinutes": 30,
            "subject": {
              "SPANISH": "una cita",
              "BASQUE": "hitzordu bat"
            },
            "urls": [
              { "id": "main", "lang": "BASQUE", "value": "https://my-admin.eus/citas/appointment123?lang=eu" },
              { "id": "main", "lang": "SPANISH", "value": "https://my-admin.eus/citas/appointment123?lang=es" }
            ]
          }
        ]
      },
      "status": {
        "code": "OK"
      }
    }
    ```

    Para la documentacion detallada de cada bloque: [:octicons-arrow-right-24: Semantica Base](../semantica/semantica-base/index.md)

!!! tip "URLs de detalle"
    Incluye siempre URLs donde la persona pueda ver más detalles, modificar o cancelar el elemento en tu web.

---

## Especificacion completa

Para la especificacion detallada de campos, validaciones y errores:

[:octicons-arrow-right-24: Semantica Data-Retrieve](../semantica/data-retrieve/index.md)

---

## Codigo de referencia

Repositorios con tests y codigo de ejemplo para Data-Retrieve y cabeceras de seguridad:

| Recurso | Repositorio |
|---------|-------------|
| Definicion de cabeceras HTTP (constantes) | [DN00InteropHeaders.java]({{ repos.common_interop_api_blob }}/denaCommonInteropAPIModelClasses/src/main/java/dena/api/common/model/interop/DN00InteropHeaders.java) |
| Mock factory para expedientes | [DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |
| Mock factory para notificaciones | [DN99DENATestMockObjFactoryForAdministrativeNotice.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java) |
| Mock factory para pagos | [DN99DENATestMockObjFactoryForOneOffPayment.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/payment/DN99DENATestMockObjFactoryForOneOffPayment.java) |
| Mock factory para citas | [DN99DENATestMockObjFactoryForScheduleItem.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/schedule/DN99DENATestMockObjFactoryForScheduleItem.java) |
| Mock factory para registros oficiales | [DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/register/DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java) |
| Campos base (oid, id, urls) | [DN99DENATestMockObjFactoryForDENADataExchangedObjectBase.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/DN99DENATestMockObjFactoryForDENADataExchangedObjectBase.java) |

!!! tip "Usa los mock factories como referencia"
    Estos tests generan objetos de ejemplo completamente validos. Puedes usarlos para ver el formato exacto esperado por DENA, incluidas las cabeceras de seguridad y la estructura del mensaje interop.

---

**Siguiente:** [:octicons-arrow-right-24: Metadata-Sync](./metadata-sync.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
