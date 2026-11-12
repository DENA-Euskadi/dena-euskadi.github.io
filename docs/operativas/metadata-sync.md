# :material-bell-ring: Metadata-Sync (Notificar Cambios)

## Concepto

Metadata-Sync es la operativa por la cual **tu administración notifica a DENA que hay datos nuevos o actualizados** para ciertas personas. Sin esto, DENA no sabe cuándo hay novedades y la app no puede avisar a la persona de que hay datos frescos.

![SRMD High Level](../adjuntos/imagenes/arquitectura/services-srmd-high-level.png)

### ¿Cómo funciona?

Tu admin **periódicamente** (tan frecuentemente como sea posible):

1. Consulta su BD buscando datos que han cambiado desde el último ciclo
2. Para cada cambio, crea un registro SRMD:
    ```
    {persona} | {tipo de dato} | {admin} | {instante última actualización}
    ```
3. Envía todos los SRMD a DENA-CORE vía REST

![SRMD Strategies](../adjuntos/imagenes/arquitectura/services-srmd-strategies.png)

### Puntos clave

- Solo envías **meta-datos** (qué cambió y cuándo), NO los datos en sí ni quién es la persona concretamente
- Puedes enviar SRMD de **múltiples tipos de dato** y **múltiples personas** en una sola llamada
- Si un dato es nuevo O se ha modificado, cuenta como cambio
- Los registros eliminados también cuentan como cambio
- La frecuencia ideal es: tan a menudo como puedas (cada minuto, cada 5 minutos, cada hora...)

---

## Implementación

### Paso 1: Consultar cambios en tu BD

La única información que DENA necesita es: **¿qué tipo de dato se ha modificado?** y **¿cuándo fue el último cambio?**

```sql
SELECT PERSON_ID,
       MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE_AT
  FROM MY_TABLE
 WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= ?
 GROUP BY PERSON_ID;
```

!!! note ""
    `COALESCE(LAST_UPDATED_AT, CREATED_AT)` usa la fecha de actualización si existe, o la de creación si no ha sido actualizado nunca.

### Paso 2: Convertir a modelo DENA

```java
private List<DN00SyncMetaDataFromAdminToCOREItem> _toSRMD(
        final Collection<DBPersonLastChange> dbChanges) {
    return dbChanges.stream()
        .map(dbChange -> new DN00SyncMetaDataFromAdminToCOREItem(
            new DN00OrgAdminRef(DN00OrgAdminID.forId("MY_ADMIN")),
            DN00PersonRef.from(DN00PersonID.forId(dbChange.personId())),
            dbChange.lastChangedAt(),
            new DN00DataTypeRef(DN00DataTypeID.forId("citizen_service_appointment")),
            null))  // null = instancia de origen por defecto (DEFAULT)
        .toList();
}
```

### Paso 3: Envolver en un interop message y enviar

```java
// Crear el interop context
DN00InteropContext ctx = DN00InteropContextBuilder
    .createMessageOfType(DN00InteropMessageType.ADMIN_SRMD_SYNC_REQ)
    .withGeneratedCorrelationOID()
    .fromAdmin(DN00OrgAdminID.forId("MY_ADMIN"))
    .withSubjectPerson(null)
    .aboutDataType(null)
    .build();

// Crear el mensaje de petición (los items se pasan directamente, sin wrapper)
DN00SyncMetaDataFromAdminRequestMessage reqMsg =
    new DN00SyncMetaDataFromAdminRequestMessage(ctx, srmdItems);

// Serializar a JSON
String json = marshaller.forWriting().toJSON(reqMsg);

// Enviar por HTTP POST
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.dena.eus/srmd/"))
    .header("Content-Type", "application/json")
    .header("Accept", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(json))
    .build();
```

### Ejemplo: JSON enviado a DENA

```json
[
  {
    "admin": { "id": "EJGV" },
    "aboutPerson": { "id": "40404040H" },
    "someDataWasUpdatedAt": "2026-08-17T15:14:07.036Z",
    "ofType": { "id": "ADMIN_NOTICE" },
    "fromDataOriginInstance": "DEFAULT"
  },
  {
    "admin": { "id": "EJGV" },
    "aboutPerson": { "id": "12345678A" },
    "someDataWasUpdatedAt": "2026-08-17T15:14:07.032Z",
    "ofType": { "id": "PROCEDURE_RECORD" },
    "fromDataOriginInstance": "DEFAULT"
  }
]
```

!!! tip "IDs vs OIDs"
    - El **id de la persona** es su NIF (ej: `12345678A`)
    - El **id de la administracion** es siempre su NIF/CIF (ej: `S4833001C` para EJGV)
    - El **id del tipo de dato** es el identificador acordado con el equipo DENA (ej: `PROCEDURE_RECORD`)
    - El **fromDataOriginInstance** es `DEFAULT` si solo tienes un origen de datos. Si tienes multiples origenes, usa el identificador previamente comunicado al equipo DENA
    
    No necesitas usar los OIDs internos de DENA. Los IDs de negocio son suficientes.

!!! note "Sobre fromDataOriginInstance"
    Si tu administracion solo tiene un origen de datos por tipo de dato (lo mas habitual), envia `"fromDataOriginInstance": "DEFAULT"` y no necesitas preocuparte de nada mas.
    
    Si tienes **multiples origenes** para el mismo tipo de dato (ej: varios gestores de expedientes), el campo `fromDataOriginInstance` debe contener el identificador del origen concreto. Este identificador se acuerda previamente con el equipo DENA durante la configuracion del conector. Si no lo comunicas de antemano, los items fallaran en el procesamiento.

### Ejemplo: respuesta de DENA

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 2,
  "processedOK": [ ... ],
  "processedNOK": [
    {
      "item": { ... },
      "error": "The [data type] with ref=unknown could NOT be validated"
    }
  ]
}
```

DENA devuelve que items se procesaron correctamente (`processedOK`) y cuales fallaron (`processedNOK`) con el motivo del error. Un envio puede tener items validos e invalidos simultaneamente: los validos se procesan con normalidad y los invalidos se rechazan individualmente sin afectar al resto.

!!! warning "Error clasico: data origin no configurado"
    El fallo mas habitual en `processedNOK` es un `fromDataOriginInstance` que no esta dado de alta en la configuracion de DENA. Si recibes errores de validacion en el data origin, verifica con el equipo DENA que el identificador que usas coincide con el configurado en el conector.

---

## Ejemplo real de llamada completa

### Petición (REQUEST) — tu admin notifica a DENA

Tu administración envía al endpoint SRMD de DENA una lista de cambios detectados. En este ejemplo se notifican 2 cambios de tipo `PERSON_DATA` para 2 personas distintas de la administración `SANDBOXCIF`:

```json
{
    "context": {
        "message": {
            "type": "ADMIN_SRMD_SYNC_REQ",
            "correlationId": "9cd46653-284c-468d-81f6-53c84bd50794",
            "interopRouteData": [
                { "denaComponentId": "ADMIN", "timestamp": "2026-11-10T08:07:38.934Z" }
            ]
        },
        "originAdmin": { "id": "SANDBOXCIF" }
    },
    "payload": [
        {
            "admin": { "id": "SANDBOXCIF" },
            "aboutPerson": { "id": "11189396B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
            "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
            "ofType": { "id": "PERSON_DATA" }
        },
        {
            "admin": { "id": "SANDBOXCIF" },
            "aboutPerson": { "id": "11111111B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
            "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
            "ofType": { "id": "PERSON_DATA" }
        }
    ]
}
```

### Respuesta (RESPONSE) — DENA acusa recibo

DENA procesa cada item individualmente y devuelve los aceptados (`processedOK`) y los rechazados (`processedNOK`) con el motivo del error:

```json
{
    "code": "OK",
    "context": {
        "message": {
            "type": "ADMIN_SRMD_SYNC_RESP",
            "correlationId": "9cd46653-284c-468d-81f6-53c84bd50794",
            "interopRouteData": [
                { "denaComponentId": "DENA_INTEROP_ADMIN_SYNC", "timestamp": "2026-11-10T08:07:39.5179823Z" },
                { "denaComponentId": "ADMIN", "timestamp": "2026-11-10T08:07:38.934Z" }
            ]
        }
    },
    "payload": {
        "transactionOid": "FFCD3371-D351-427E-A890-0C886BCA1275",
        "receivedItemsCount": 2,
        "processedOK": [
            {
                "admin": { "id": "SANDBOXCIF", "oid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011" },
                "dataLastUpdatedAt": "2026-11-09T20:41:39.8825952Z",
                "dataOriginInstanceId": "DEFAULT",
                "dataType": { "id": "PERSON_DATA", "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4007" },
                "entityVersion": 5,
                "numericId": 0,
                "lastTransactionOid": "FFCD3371-D351-427E-A890-0C886BCA1275",
                "oid": "6485BF2C-5C53-4EC0-ABE3-212DD4403913",
                "person": { "id": "11189396B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
                "trackingInfo": {
                    "createdAt": "2026-11-04T17:22:41.6877234Z",
                    "lastUpdatedAt": "2026-11-10T08:07:39.4321202Z",
                    "lastUpdaterSecurityPrincipalOid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011"
                }
            }
        ],
        "processedNOK": [
            {
                "item": {
                    "admin": { "id": "SANDBOXCIF", "oid": "6d9d73d4-3d3d-6d3d-ad3d-3d3d3d3d3011" },
                    "aboutPerson": { "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
                    "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
                    "ofType": { "id": "PERSON_DATA", "oid": "7e0e84e5-4e4e-7e4e-be4e-4e4e4e4e4007" },
                    "fromDataOriginInstance": "DEFAULT"
                },
                "error": "The [person] with ref=631011F3-9A2F-4D08-9F56-B7F4D6A0749E;null could NOT be validated"
            }
        ]
    }
}
```

!!! info "Estructura del mensaje"
    - **`context.message.type`**: `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP`
    - **`payload[]`** (petición): array de SRMD items (admin + persona + tipo de dato + timestamp)
    - **`payload.receivedItemsCount`** (respuesta): total de items recibidos
    - **`payload.processedOK[]`**: items aceptados con sus metadatos de seguimiento
    - **`payload.processedNOK[]`**: items rechazados con el motivo del error

---

## Recursos disponibles

| Recurso | Descripción |
|---------|-------------|
| :material-api: [Swagger UI — admin-sync-rest](https://api-batera.pre.dena.eus/sync/swagger-ui/index.html) | API interactiva del servicio que recibe los SRMD (entorno **PRE**). Permite explorar endpoints y probar llamadas. OpenAPI JSON: [`/sync/v3/api-docs`](https://api-batera.pre.dena.eus/sync/v3/api-docs) |
| :material-code-json: [JSON Schema de los mensajes REST](../adjuntos/json-schema.md) | Esquema formal y validable de los mensajes (incluye `DN00SyncMetaDataFromAdminRequest/ResponseMessage`) |
| :material-rocket-launch: [Recursos Postman](../adjuntos/index.md) | Colección y environment con requests de ejemplo de Metadata-Sync |
| :material-folder-table: [Todos los recursos Swagger](../adjuntos/swagger.md) | Índice de las Swagger UI y especificaciones OpenAPI de todos los servicios |

!!! info "Entorno de referencia"
    La documentación refleja el entorno **PRE**.

---

## Especificación completa

Para la especificación detallada del endpoint, modelo y errores:

[:octicons-arrow-right-24: Semántica Metadata-Sync](../semantica/metadata-sync/index.md)

---

**Siguiente:** [:octicons-arrow-right-24: Person-Sync](./person-sync.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
