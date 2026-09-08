# DATA-RETRIEVE Endpoint-a — Administrazioentzako Espezifikazioa

## Endpoint-a

```
POST /api/retrieveData
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

---

## Eskaera

> Testuinguruaren Java klasea: [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": [
        { "denaComponentId": "apiGateway", "timestamp": "2024-06-01T10:00:00Z" }
      ]
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DATATYPE-OID-001" },
    "originClientInstallment": "CLIENT-INSTALLMENT-OID-001",
    "destinationAdmin": { "oid": "ADMIN-OID-001", "id": "ADMIN-001", "dir3Id": "EA0000001" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
  },
  "protocol": {
    "urls": [],
    "timeOut": "30s"
  },
  "payload": {
    "person": "PERSON-OID-001"
  }
}
```

| Eremua | Nahitaezkoa | Deskribapena |
|-------|:-----------:|-------------|
| `context.message.type` | ✅ | Mezu-mota (`PERSON_FETCH_DATA`). Ikusi [`DN00InteropMessageType`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageType.java) |
| `context.message.correlationId` | ✅ | Trazabilitaterako korrelazio UUID-a. Ikusi [`DN00InteropMessageData`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropMessageData.java) |
| `context.message.interopRouteData` | ❌ | DENA osagaien traza. Ikusi [`DN00IteropRouteDataItem`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00IteropRouteDataItem.java) |
| `context.dataType.id` | ✅ | Datu-mota (marshallTypeId): `administrativeServiceProcedureRecord`, `administrativeNotice`, `administrativeOfficialRegisterRecord`, `oneOffPayment`, `directDebitPayment`, `scheduleItem`, `personData`. Ikusi [`DN00DataTypeEnum`]({{ repos.common_data_api_blob }}/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/DN00DataTypeEnum.java) |
| `context.dataType.oid` | ❌ | Datu-motaren OID-a (DataTypeRef). Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.originClientInstallment` | ❌ | Jatorrizko bezero-instalazioaren OID-a (mezua bezero-gailu batek bidaltzen duenean). Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin.oid` | ❌ | Helmugako administrazioaren OID-a (OrgAdminRef). Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin.id` | ❌ | Helmugako administrazioaren identifikatzailea. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.destinationAdmin.dir3Id` | ❌ | Helmugako administrazioaren DIR3 kodea. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.subjectPerson.id` | ✅ | Pertsonaren DNI/NIE/NIF (PersonRef). Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `context.subjectPerson.oid` | ❌ | Pertsonaren OID-a. Ikusi [`DN00InteropContext`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropContext.java) |
| `protocol.urls` | ❌ | Protokoloaren txantiloi URLak. Ikusi [`DN00InteropProtocol`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropProtocol.java) |
| `protocol.timeOut` | ❌ | Timeout-a (adib.: `"30s"`). Ikusi [`DN00InteropProtocol`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropProtocol.java) |
| `payload` | ✅ | Eskaeraren payload-a |

> Fluxuaren norabidea (`REQUEST`/`RESPONSE`) mezu-motatik eratorritako balioa da eta ez da JSON-ean serializatzen. Ikusi [`DN00InteropFlowDirection`]({{ repos.common_api_blob }}/denaCommonAPIModelClasses/src/main/java/dena/api/common/interop/context/DN00InteropFlowDirection.java)

---

## Erantzun arrakastatsua (HTTP 200)

> Erantzunaren egoera: `code` (`DN00InteropResponseStatus`), `errorId` eta `details`. Ikusi [Status](../semantica-base/modelo/status.md)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DATATYPE-OID-001" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" },
    "destinationAdmin": { "oid": "ADMIN-OID-001", "id": "ADMIN-001" }
  },
  "payload": {
    "dataItems": [
      {
        "type": "administrativeServiceProcedureRecord",
        "oid": "EXP-OID-001",
        "id": "EXP-2024-00123",
        "service": {
          "serviceNameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
          "originRef": { "id": "SRV-LIC-ACT" }
        },
        "procedure": {
          "serviceNameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
          "originRef": { "id": "PROC-LIC-APER" }
        },
        "createdAt": "2024-03-15T10:30:00Z",
        "lastUpdatedAt": "2024-06-01T14:00:00Z",
        "applicationDate": "2024-03-14T09:00:00Z",
        "regNumber": "REG-2024-00123",
        "interested": { "partyId": "12345678A", "partyName": "Juan García" },
        "state": {
          "stateCode": "IN_PROGRESS",
          "description": { "SPANISH": "En tramitación", "BASQUE": "Izapidetzen" }
        },
        "urls": [
          { "url": "https://sede.miadmin.eus/expediente/EXP-2024-00123", "language": "SPANISH", "tags": ["default"] }
        ]
      }
    ]
  },
  "code": "OK",
  "errorId": null,
  "details": null
}
```

## Daturik gabeko erantzuna (HTTP 200)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DATATYPE-OID-001" },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
  },
  "payload": { "dataItems": [] },
  "code": "OK"
}
```

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
  },
  "payload": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Persona no encontrada en el sistema" }
}
```

### Egoera-kodeak (`code`)

| Kodea | Deskribapena |
|--------|-------------|
| `OK` | Mezua behar bezala prozesatua |
| `CLIENT_ERR` | Bezeroaren errorea (eskaera gaizki osatua, pertsona ez da aurkitu) |
| `SERVER_ERR` | Zerbitzariaren errorea (barne-errorea) |
| `QUEUED` | Mezua ilaran prozesamendu asinkronorako |

---

## Objektu-motak `dataItems`-en

`dataItems` arrayaren elementu bakoitza [eremu komunak](./data/campos-comunes.md) (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) heredatzen dituen objektu bat da eta eremu espezifikoak gehitzen ditu bere motaren arabera:

| `type` | Objektua | Dokumentazioa |
|--------|--------|---------------|
| `administrativeServiceProcedureRecord` | Espedientea | [expediente.md](./data/expediente.md) |
| `administrativeNotice` | Jakinarazpena | [notificacion.md](./data/notificacion.md) |
| `administrativeOfficialRegisterRecord` | Erregistro ofiziala | [registro-oficial.md](./data/registro-oficial.md) |
| `oneOffPayment` | Ordainketa bakarra | [pago.md](./data/pago.md) |
| `directDebitPayment` | Helbideratze bankarioa | [pago.md](./data/pago.md) |
| `scheduleItem` | Hitzordua | [cita.md](./data/cita.md) |

Erreferentziatutako objektu lagungarriak:

| Objektua | Dokumentazioa |
|--------|---------------|
| Zerbitzua / Prozedura | [servicio-administrativo.md](./data/servicio-administrativo.md) |
| Unitate Organikoa | [unidad-organica.md](./data/unidad-organica.md) |
| Eremu komunak (oinarria) | [campos-comunes.md](./data/campos-comunes.md) |

---

## Adibidea — Jakinarazpena

```json
{
  "type": "OFFICIAL_NOTICE",
  "oid": "NOT-OID-001",
  "id": "NOT-2024-00456",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "issuedAt": "2024-05-20T09:00:00Z",
  "readedAt": null,
  "state": "PENDING_TO_BE_READED_BY_DESTINATION",
  "actSubjectByLanguage": { "SPANISH": "Resolución de concesión de ayuda", "BASQUE": "Laguntza emateko ebazpena" },
  "urls": [{ "url": "https://sede.miadmin.eus/notificacion/NOT-2024-00456", "language": "SPANISH", "tags": ["default"] }]
}
```

## Adibidea — Ordainketa bakarra

```json
{
  "type": "oneOffPayment",
  "oid": "PAY-OID-001",
  "id": "PAY-2024-00321",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "paymentType": "ONE_OFF_PAYMENT",
  "paymentSubjectByLanguage": { "SPANISH": "Tasa por licencia de actividad", "BASQUE": "Jarduera-lizentziaren tasa" },
  "paymentDates": { "dueDate": "2024-06-30", "surchargedAt": "2024-07-15", "paidAt": null },
  "format": "502",
  "amount": { "amount": 45.50, "currency": "EUR" },
  "amountIfSurcharged": { "amount": 50.05, "currency": "EUR" },
  "data": { "forStatus": "PENDING", "at": null, "medium": null, "device": null },
  "urls": [{ "url": "https://pago.miadmin.eus/pay/PAY-2024-00321", "language": "SPANISH", "tags": ["payment"] }]
}
```

## Adibidea — Erregistro ofiziala

```json
{
  "type": "administrativeOfficialRegisterRecord",
  "oid": "REG-OID-001",
  "id": "REG-2024-00789",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "registeredAt": "2024-04-10T08:30:00Z",
  "subjectByLanguage": { "SPANISH": "Solicitud de licencia de obras", "BASQUE": "Obra-lizentzia eskaera" },
  "state": { "stateCode": "PRESENTED", "description": { "SPANISH": "Presentado", "BASQUE": "Aurkeztua" } }
}
```

## Adibidea — Hitzordua

```json
{
  "type": "scheduleItem",
  "oid": "SCHED-OID-001",
  "id": "CITA-2024-00050",
  "year": 2024,
  "monthOfYear": 7,
  "dayOfMonth": 15,
  "hourOfDay": 10,
  "minuteOfHour": 30,
  "durationMinutes": 30,
  "priority": "NORMAL",
  "subject": { "SPANISH": "Cita previa para renovación de DNI", "BASQUE": "NAN berritzeko aurretiko hitzordua" },
  "location": {
    "country": { "id": "ES", "name": "España" },
    "administrativeAreaLevel1": { "id": "PV", "name": "País Vasco" },
    "administrativeAreaLevel3": { "id": "48020", "name": "Bilbao" },
    "zipCode": "48001",
    "address": "Gran Vía 50, Bilbao"
  }
}
```

## Adibidea — Helbideratze bankarioa

```json
{
  "type": "directDebitPayment",
  "oid": "DD-OID-001",
  "id": "DD-2024-00100",
  "procedureRecord": { "oid": "EXP-OID-001", "id": "EXP-2024-00123" },
  "paymentType": "DIRECT_DEBIT",
  "paymentSubjectByLanguage": { "SPANISH": "Cuota mensual guardería", "BASQUE": "Haur-eskolako hileko kuota" },
  "directDebitData": {
    "startDate": "2024-01-15",
    "expiresAt": null,
    "frequency": "MONTHLY",
    "medium": "DIRECT_DEBIT",
    "mediumHint": "2100 ***** 051332"
  },
  "nextChargeAt": "2024-07-01",
  "nextChargeAmountInEuro": 120.00,
  "paymentStatus": "ACTIVE",
  "history": [
    { "at": "2024-06-01", "amountInEuro": 120.00 }
  ]
}
```

---

## Autentifikazioa

Administrazioak OAuth2 behar badu, goiburu hau jasoko du:

```
Authorization: Bearer <access_token>
```

Tokena automatikoki lortzen da client credentials bidez.

---

## HTTP kodeak

| Kodea | Esanahia |
|--------|-------------|
| `200` | Datuak behar bezala itzuliak (zerrenda hutsa izan daiteke) |
| `400` | Eskaera gaizki osatua edo parametro baliogabeak |
| `401` | Baimenik gabe (token baliogabea edo iraungitua) |
| `403` | Debekatua (baimenik gabe) |
| `404` | Pertsona ez da aurkitu |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

---

## Administrazioarentzako eskakizunak

1. `application/json` onartzen eta itzultzen duen `POST` endpoint bat eskaini
2. `context.subjectPerson.id` interpretatu pertsona identifikatzeko
3. `context.dataType.id` interpretatu datu-mota iragazteko
4. Objektuak eredu semantikoaren formatuan itzuli
5. Hizkuntza anitzeko testuak sartu (gaztelania eta euskara gutxienez)
6. Egoitza elektronikorako URLak sartu ahal denean
7. HTTP kode estandarrak errespetatu
8. HTTP 200 itzuli `dataItems: []`-rekin daturik ez dagoenean (ez erabili 404)
9. 30 segundo baino gutxiagoan erantzun
10. `code: "OK"` erabili erantzun arrakastatsuetan eta `code: "CLIENT_ERR"` edo `code: "SERVER_ERR"` erroreetan

---

## Lotutako dokumentazioa

| Dokumentua | Edukia |
|-----------|----------|
| [campos-comunes.md](./data/campos-comunes.md) | Objektu guztiek heredatzen dituzten eremuak (`oid`, `id`, `urls`, `originAdmin`, `aboutPerson`) |
| [expediente.md](./data/expediente.md) | Espediente administratiboa |
| [notificacion.md](./data/notificacion.md) | Jakinarazpena / komunikazioa |
| [registro-oficial.md](./data/registro-oficial.md) | Erregistro-sarrera |
| [pago.md](./data/pago.md) | Ordainketa bakarra eta helbideratze bankarioa |
| [cita.md](./data/cita.md) | Aurretiko hitzordua / agenda-elementua |
| [servicio-administrativo.md](./data/servicio-administrativo.md) | Zerbitzua eta prozedura |
| [unidad-organica.md](./data/unidad-organica.md) | Unitate organikoa |
| [validaciones.md](./validaciones.md) | Formatu eta balioztapen arauak |
| [errores-troubleshooting.md](./errores-troubleshooting.md) | Errore arrunten gida |










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
