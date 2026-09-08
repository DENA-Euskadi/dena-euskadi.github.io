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

        Map<String, Object> message = (Map<String, Object>) context.get("message");
        String personId = (String) subjectPerson.get("id");
        String dataTypeId = (String) dataType.get("id");

        // Buscar datos según tipo
        List<Map<String, Object>> dataItems = switch (dataTypeId) {
            case "administrativeServiceProcedureRecord"  -> fetchRecords(personId);
            case "administrativeNotice"                  -> fetchNotices(personId);
            case "administrativeOfficialRegisterRecord"  -> fetchRegistry(personId);
            case "oneOffPayment"                         -> fetchPayments(personId);
            case "scheduleItem"                          -> fetchSchedule(personId);
            case "personData"                            -> fetchPersonData(personId);
            default                                      -> List.of();
        };

        // Construir response
        Map<String, Object> response = Map.of(
            "context", Map.of(
                "message", Map.of(
                    "type", message.get("type"),
                    "correlationId", message.get("correlationId"),
                    "interopRouteData", message.getOrDefault("interopRouteData", List.of())
                ),
                "dataType", dataType,
                "subjectPerson", subjectPerson
            ),
            "payload", Map.of("dataItems", dataItems),
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
    var message = context.GetProperty("message");
    var personId = context.GetProperty("subjectPerson").GetProperty("id").GetString();
    var dataTypeId = context.GetProperty("dataType").GetProperty("id").GetString();

    var dataItems = dataTypeId switch
    {
        "administrativeServiceProcedureRecord"  => GetRecords(personId),
        "administrativeNotice"                  => GetNotices(personId),
        "administrativeOfficialRegisterRecord"  => GetRegistry(personId),
        "oneOffPayment"                         => GetPayments(personId),
        "scheduleItem"                          => GetSchedule(personId),
        "personData"                            => GetPersonData(personId),
        _                                       => new List<object>()
    };

    return Results.Ok(new
    {
        context = new
        {
            message = new
            {
                type = message.GetProperty("type").GetString(),
                correlationId = message.GetProperty("correlationId").GetString(),
                interopRouteData = new List<object>()
            },
            dataType = new { id = dataTypeId },
            subjectPerson = new { id = personId }
        },
        payload = new { dataItems },
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
    message = context["message"]
    person_id = context["subjectPerson"]["id"]
    data_type_id = context["dataType"]["id"]

    fetchers = {
        "administrativeServiceProcedureRecord": fetch_records,
        "administrativeNotice": fetch_notices,
        "administrativeOfficialRegisterRecord": fetch_registry,
        "oneOffPayment": fetch_payments,
        "scheduleItem": fetch_schedule,
        "personData": fetch_person_data,
    }

    data_items = fetchers.get(data_type_id, lambda _: [])(person_id)

    return {
        "context": {
            "message": {
                "type": message.get("type"),
                "correlationId": message.get("correlationId"),
                "interopRouteData": message.get("interopRouteData", []),
            },
            "dataType": {"id": data_type_id},
            "subjectPerson": {"id": person_id},
        },
        "payload": {"dataItems": data_items},
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
  const { message } = context;
  const personId = context.subjectPerson.id;
  const dataTypeId = context.dataType.id;

  const fetchers = {
    administrativeServiceProcedureRecord: fetchRecords,
    administrativeNotice: fetchNotices,
    administrativeOfficialRegisterRecord: fetchRegistry,
    oneOffPayment: fetchPayments,
    scheduleItem: fetchSchedule,
    personData: fetchPersonData,
  };

  const dataItems = (fetchers[dataTypeId] || (() => []))(personId);

  res.json({
    context: {
      message: {
        type: message.type,
        correlationId: message.correlationId,
        interopRouteData: message.interopRouteData || [],
      },
      dataType: { id: dataTypeId },
      subjectPerson: { id: personId },
    },
    payload: { dataItems },
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
    $message = $context['message'];
    $personId = $context['subjectPerson']['id'];
    $dataTypeId = $context['dataType']['id'];

    $dataItems = match ($dataTypeId) {
        'administrativeServiceProcedureRecord'  => fetchRecords($personId),
        'administrativeNotice'                  => fetchNotices($personId),
        'administrativeOfficialRegisterRecord'  => fetchRegistry($personId),
        'oneOffPayment'                         => fetchPayments($personId),
        'scheduleItem'                          => fetchSchedule($personId),
        'personData'                            => fetchPersonData($personId),
        default                                 => [],
    };

    return response()->json([
        'context' => [
            'message' => [
                'type' => $message['type'],
                'correlationId' => $message['correlationId'],
                'interopRouteData' => $message['interopRouteData'] ?? [],
            ],
            'dataType' => ['id' => $dataTypeId],
            'subjectPerson' => ['id' => $personId],
        ],
        'payload' => ['dataItems' => $dataItems],
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
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": []
    },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-0001" }
  },
  "payload": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Person not found in the system" }
}
```

### Java — Error handling

```java
@ExceptionHandler(PersonNotFoundException.class)
public ResponseEntity<Map<String, Object>> handleNotFound(PersonNotFoundException ex,
                                                          HttpServletRequest request) {
    Map<String, Object> response = Map.of(
        "context", Map.of(
            "message", Map.of(
                "type", "PERSON_FETCH_DATA"
            )
        ),
        "payload", (Object) null,  // explicit null
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
            "context": {"message": {"type": "PERSON_FETCH_DATA"}},
            "payload": None,
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
    context: { message: { type: 'PERSON_FETCH_DATA' } },
    payload: null,
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
