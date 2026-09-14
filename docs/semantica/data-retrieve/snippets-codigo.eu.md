# Kode Snippetak — Endpoint-aren Inplementazioa

## Deskribapena

Kode-adibideak `POST /api/retrieveData` endpoint-a programazio-lengoaia ezberdinetan inplementatzeko. Snippet bakoitzak eskaera nola jaso (formatu murriztua: `context` `subjectPerson`, `dataType` eta `administration`-ekin), eremu nagusiak nola atera eta erantzuna DENAk espero duen formatuan nola itzuli erakusten du (`code` erro-mailan eta `payload.dataItems`, non elementu bakoitzak objektua `data`-n biltzen duen).

> Kontratu osoa (benetako eskaera eta erantzuna): [endpoint-data-retrieve.md](./endpoint-data-retrieve.md)

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
        // Testuinguruaren eremuak atera (formatu murriztua)
        Map<String, Object> context = (Map<String, Object>) request.get("context");
        Map<String, Object> subjectPerson = (Map<String, Object>) context.get("subjectPerson");
        Map<String, Object> dataType = (Map<String, Object>) context.get("dataType");

        String personId = (String) subjectPerson.get("id");
        String dataTypeId = (String) dataType.get("id");

        // Datuak motaren arabera bilatu; elementu bakoitza "data"-n biltzen da
        List<Map<String, Object>> dataItems = switch (dataTypeId) {
            case "administrativeServiceProcedureRecord"  -> fetchRecords(personId);
            case "administrativeNotice"                  -> fetchNotices(personId);
            case "administrativeOfficialRegisterRecord"  -> fetchRegistry(personId);
            case "oneOffPayment"                         -> fetchPayments(personId);
            case "scheduleItem"                          -> fetchSchedule(personId);
            case "personData"                            -> fetchPersonData(personId);
            default                                      -> List.of();
        };

        // Erantzuna eraiki: code erro-mailan, payload.dataItems
        Map<String, Object> response = Map.of(
            "context", context,
            "code", "OK",
            "payload", Map.of("dataItems", dataItems)
        );

        return ResponseEntity.ok(response);
    }

    private List<Map<String, Object>> fetchRecords(String personId) {
        // Pertsonaren espedienteak administrazioaren sisteman kontsultatu.
        // Negozio-objektu bakoitza "data"-n bilduta itzultzen da.
        return List.of(
            Map.of("data", Map.of(
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
            ))
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
            subjectPerson = new { id = personId },
            dataType = new { id = dataTypeId }
        },
        code = "OK",
        payload = new { dataItems }
    });
});

app.Run();
```

---

## Python (FastAPI)

```python
from fastapi import FastAPI
from typing import Any

app = FastAPI()

@app.post("/api/retrieveData")
async def retrieve_data(request: dict[str, Any]) -> dict[str, Any]:
    context = request["context"]
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
            "subjectPerson": {"id": person_id},
            "dataType": {"id": data_type_id},
        },
        "code": "OK",
        "payload": {"dataItems": data_items},
    }


def fetch_records(person_id: str) -> list[dict]:
    # Negozio-objektu bakoitza "data"-n biltzen da
    return [
        {
            "data": {
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
      subjectPerson: { id: personId },
      dataType: { id: dataTypeId },
    },
    code: 'OK',
    payload: { dataItems },
  });
});

function fetchRecords(personId) {
  // Negozio-objektu bakoitza "data"-n biltzen da
  return [
    {
      data: {
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
            'subjectPerson' => ['id' => $personId],
            'dataType' => ['id' => $dataTypeId],
        ],
        'code' => 'OK',
        'payload' => ['dataItems' => $dataItems],
    ]);
});

function fetchRecords(string $personId): array {
    // Negozio-objektu bakoitza "data"-n biltzen da
    return [
        [
            'data' => [
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
        ],
    ];
}
```

---

## Erroreen kudeaketa (lengoaia guztietan)

Errore-erantzunak `code`-a erro-mailan darama (`CLIENT_ERR`/`SERVER_ERR`), `errorId` eta `details` aukerakoekin:

```json
{
  "context": {
    "subjectPerson": { "id": "12345678A" },
    "dataType": { "id": "administrativeServiceProcedureRecord" }
  },
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
    code: statusCode < 500 ? 'CLIENT_ERR' : 'SERVER_ERR',
    errorId: err.errorId || 'INTERNAL_ERROR',
    details: { details: err.message },
  });
});
```

---

## Lotutako dokumentazioa

- [endpoint-data-retrieve.md](./endpoint-data-retrieve.md) — Endpoint-aren kontratu osoa
- [validaciones.md](./validaciones.md) — Formatu- eta balidazio-arauak
- [errores-troubleshooting.md](./errores-troubleshooting.md) — Ohiko erroreen gida

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
