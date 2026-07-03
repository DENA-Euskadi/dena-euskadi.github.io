# Code Snippets — Endpoint Implementation

## Description

Code examples to implement the `POST /api/retrieveData` endpoint in different programming languages. Each snippet shows how to receive the request, extract the key fields from the context and return the response in the format expected by DENA.

---

## Java (Spring Boot)

```java
@RestController
@RequestMapping("/api")
public class RetrieveDataController {

    @PostMapping(value = "/retrieveData", 
                 consumes = MediaType.APPLICATION_JSON_VALUE,
                 produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<Map<String, Object>> retrieveData(@RequestBody Map<String, Object> request) {
        // Extraer contexto
        Map<String, Object> context = (Map<String, Object>) request.get("context");
        Map<String, Object> subjectPerson = (Map<String, Object>) context.get("subjectPerson");
        Map<String, Object> dataType = (Map<String, Object>) context.get("dataType");

        String personId = (String) subjectPerson.get("personId");
        String dataTypeId = (String) dataType.get("dataTypeId");

        // Buscar datos según tipo
        List<Map<String, Object>> dataItems = switch (dataTypeId) {
            case "RECORDS"     -> fetchRecords(personId);
            case "NOTICES"     -> fetchNotices(personId);
            case "REGISTER"    -> fetchRegistry(personId);
            case "PAYMENTS"    -> fetchPayments(personId);
            case "SCHEDULE"    -> fetchSchedule(personId);
            case "PERSON_DATA" -> fetchPersonData(personId);
            default            -> List.of();
        };

        // Construir response
        Map<String, Object> response = Map.of(
            "context", Map.of(
                "messageType", context.get("messageType"),
                "dataType", dataType,
                "messageCorrelationId", context.get("messageCorrelationId"),
                "flowDirection", "RESPONSE",
                "subjectPerson", subjectPerson
            ),
            "data", Map.of("dataItems", dataItems),
            "code", "OK"
        );

        return ResponseEntity.ok(response);
    }

    private List<Map<String, Object>> fetchRecords(String personId) {
        // Consultar expedientes de la persona en el sistema de la administración
        return List.of(
            Map.of(
                "type", "administrativeServiceProcedureRecord",
                "oid", "EXP-OID-001",
                "id", "EXP-2024-00123",
                "service", Map.of(
                    "serviceNameByLanguage", Map.of("SPANISH", "Licencias de actividad", "BASQUE", "Jarduera-lizentziak"),
                    "originRef", Map.of("id", "SRV-LIC-ACT")
                ),
                "procedure", Map.of(
                    "serviceNameByLanguage", Map.of("SPANISH", "Solicitud de licencia", "BASQUE", "Lizentzia eskaera"),
                    "originRef", Map.of("id", "PROC-LIC-001")
                ),
                "createdAt", "2024-03-15T10:30:00Z",
                "state", Map.of(
                    "stateCode", "IN_PROGRESS",
                    "description", Map.of("SPANISH", "En tramitación", "BASQUE", "Izapidetzen")
                )
            )
        );
    }
}
```

---

## C# (.NET 8 Minimal API)

```csharp
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

app.MapPost("/api/retrieveData", async (HttpContext http) =>
{
    var request = await http.Request.ReadFromJsonAsync<JsonElement>();
    var context = request.GetProperty("context");
    var personId = context.GetProperty("subjectPerson").GetProperty("personId").GetString();
    var dataTypeId = context.GetProperty("dataType").GetProperty("dataTypeId").GetString();

    var dataItems = dataTypeId switch
    {
        "RECORDS"     => GetRecords(personId),
        "NOTICES"     => GetNotices(personId),
        "REGISTER"    => GetRegistry(personId),
        "PAYMENTS"    => GetPayments(personId),
        "SCHEDULE"    => GetSchedule(personId),
        "PERSON_DATA" => GetPersonData(personId),
        _             => new List<object>()
    };

    return Results.Ok(new
    {
        context = new
        {
            messageType = context.GetProperty("messageType").GetString(),
            dataType = new { dataTypeId },
            messageCorrelationId = context.GetProperty("messageCorrelationId").GetString(),
            flowDirection = "RESPONSE",
            subjectPerson = new { personId }
        },
        data = new { dataItems },
        code = "OK"
    });
});

app.Run();
```

---

## Python (FastAPI)

```python
from fastapi import FastAPI
from pydantic import BaseModel
from typing import Any

app = FastAPI()

@app.post("/api/retrieveData")
async def retrieve_data(request: dict[str, Any]) -> dict[str, Any]:
    context = request["context"]
    person_id = context["subjectPerson"]["personId"]
    data_type_id = context["dataType"]["dataTypeId"]

    fetchers = {
        "RECORDS": fetch_records,
        "NOTICES": fetch_notices,
        "REGISTER": fetch_registry,
        "PAYMENTS": fetch_payments,
        "SCHEDULE": fetch_schedule,
        "PERSON_DATA": fetch_person_data,
    }

    data_items = fetchers.get(data_type_id, lambda _: [])(person_id)

    return {
        "context": {
            "messageType": context.get("messageType"),
            "dataType": {"dataTypeId": data_type_id},
            "messageCorrelationId": context.get("messageCorrelationId"),
            "flowDirection": "RESPONSE",
            "subjectPerson": {"personId": person_id},
        },
        "data": {"dataItems": data_items},
        "code": "OK",
    }


def fetch_records(person_id: str) -> list[dict]:
    return [
        {
            "type": "administrativeServiceProcedureRecord",
            "oid": "EXP-OID-001",
            "id": "EXP-2024-00123",
            "service": {
                "serviceNameByLanguage": {"SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak"},
                "originRef": {"id": "SRV-LIC-ACT"},
            },
            "procedure": {
                "serviceNameByLanguage": {"SPANISH": "Solicitud de licencia", "BASQUE": "Lizentzia eskaera"},
                "originRef": {"id": "PROC-LIC-001"},
            },
            "createdAt": "2024-03-15T10:30:00Z",
            "state": {
                "stateCode": "IN_PROGRESS",
                "description": {"SPANISH": "En tramitación", "BASQUE": "Izapidetzen"},
            },
        }
    ]
```

---

## Node.js (Express)

```javascript
const express = require('express');
const app = express();
app.use(express.json());

app.post('/api/retrieveData', (req, res) => {
  const { context } = req.body;
  const personId = context.subjectPerson.personId;
  const dataTypeId = context.dataType.dataTypeId;

  const fetchers = {
    RECORDS: fetchRecords,
    NOTICES: fetchNotices,
    REGISTER: fetchRegistry,
    PAYMENTS: fetchPayments,
    SCHEDULE: fetchSchedule,
    PERSON_DATA: fetchPersonData,
  };

  const dataItems = (fetchers[dataTypeId] || (() => []))(personId);

  res.json({
    context: {
      messageType: context.messageType,
      dataType: { dataTypeId },
      messageCorrelationId: context.messageCorrelationId,
      flowDirection: 'RESPONSE',
      subjectPerson: { personId },
    },
    data: { dataItems },
    code: 'OK',
  });
});

function fetchRecords(personId) {
  return [
    {
      type: 'administrativeServiceProcedureRecord',
      oid: 'EXP-OID-001',
      id: 'EXP-2024-00123',
      service: {
        serviceNameByLanguage: { SPANISH: 'Licencias de actividad', BASQUE: 'Jarduera-lizentziak' },
        originRef: { id: 'SRV-LIC-ACT' },
      },
      procedure: {
        serviceNameByLanguage: { SPANISH: 'Solicitud de licencia', BASQUE: 'Lizentzia eskaera' },
        originRef: { id: 'PROC-LIC-001' },
      },
      createdAt: '2024-03-15T10:30:00Z',
      state: {
        stateCode: 'IN_PROGRESS',
        description: { SPANISH: 'En tramitación', BASQUE: 'Izapidetzen' },
      },
    },
  ];
}

app.listen(8080);
```

---

## PHP (Laravel)

```php
<?php

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::post('/api/retrieveData', function (Request $request) {
    $context = $request->input('context');
    $personId = $context['subjectPerson']['personId'];
    $dataTypeId = $context['dataType']['dataTypeId'];

    $dataItems = match ($dataTypeId) {
        'RECORDS'     => fetchRecords($personId),
        'NOTICES'     => fetchNotices($personId),
        'REGISTER'    => fetchRegistry($personId),
        'PAYMENTS'    => fetchPayments($personId),
        'SCHEDULE'    => fetchSchedule($personId),
        'PERSON_DATA' => fetchPersonData($personId),
        default       => [],
    };

    return response()->json([
        'context' => [
            'messageType' => $context['messageType'],
            'dataType' => ['dataTypeId' => $dataTypeId],
            'messageCorrelationId' => $context['messageCorrelationId'],
            'flowDirection' => 'RESPONSE',
            'subjectPerson' => ['personId' => $personId],
        ],
        'data' => ['dataItems' => $dataItems],
        'code' => 'OK',
    ]);
});

function fetchRecords(string $personId): array {
    return [
        [
            'type' => 'administrativeServiceProcedureRecord',
            'oid' => 'EXP-OID-001',
            'id' => 'EXP-2024-00123',
            'service' => [
                'serviceNameByLanguage' => ['SPANISH' => 'Licencias de actividad', 'BASQUE' => 'Jarduera-lizentziak'],
                'originRef' => ['id' => 'SRV-LIC-ACT'],
            ],
            'procedure' => [
                'serviceNameByLanguage' => ['SPANISH' => 'Solicitud de licencia', 'BASQUE' => 'Lizentzia eskaera'],
                'originRef' => ['id' => 'PROC-LIC-001'],
            ],
            'createdAt' => '2024-03-15T10:30:00Z',
            'state' => [
                'stateCode' => 'IN_PROGRESS',
                'description' => ['SPANISH' => 'En tramitación', 'BASQUE' => 'Izapidetzen'],
            ],
        ],
    ];
}
```

---

## Error handling (all languages)

The error response must follow this structure:

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Persona no encontrada en el sistema" }
}
```

### Java — Error handling

```java
@ExceptionHandler(PersonNotFoundException.class)
public ResponseEntity<Map<String, Object>> handleNotFound(PersonNotFoundException ex,
                                                          HttpServletRequest request) {
    Map<String, Object> response = Map.of(
        "context", Map.of(
            "messageType", "PERSON_FETCH_DATA",
            "flowDirection", "RESPONSE"
        ),
        "data", (Object) null,  // null explícito
        "code", "CLIENT_ERR",
        "errorId", "PERSON_NOT_FOUND",
        "details", Map.of("details", ex.getMessage())
    );
    return ResponseEntity.status(404).body(response);
}
```

### Python — Error handling

```python
from fastapi import HTTPException
from fastapi.responses import JSONResponse

@app.exception_handler(HTTPException)
async def handle_error(request, exc):
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "context": {"messageType": "PERSON_FETCH_DATA", "flowDirection": "RESPONSE"},
            "data": None,
            "code": "CLIENT_ERR" if exc.status_code < 500 else "SERVER_ERR",
            "errorId": "PERSON_NOT_FOUND" if exc.status_code == 404 else "INTERNAL_ERROR",
            "details": {"details": exc.detail},
        },
    )
```

### Node.js — Error handling

```javascript
app.use((err, req, res, next) => {
  const statusCode = err.statusCode || 500;
  res.status(statusCode).json({
    context: { messageType: 'PERSON_FETCH_DATA', flowDirection: 'RESPONSE' },
    data: null,
    code: statusCode < 500 ? 'CLIENT_ERR' : 'SERVER_ERR',
    errorId: err.errorId || 'INTERNAL_ERROR',
    details: { details: err.message },
  });
});
```

---

## Related documentation

- [endpoint-data-retrieve.md](./endpoint-data-retrieve.md) — Full endpoint contract
- [validaciones.md](./validaciones.md) — Format and validation rules
- [errores-troubleshooting.md](./errores-troubleshooting.md) — Common errors guide










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
