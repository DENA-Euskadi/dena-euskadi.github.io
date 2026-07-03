# Endpoint Person Push To Admin — Administrazioentzako Zehaztapena

## Endpoint

```
POST /api/person/push
Content-Type: application/json
Accept: application/json
Authorization: Bearer <token> (OAuth konfiguratuta badago)
```

---

## Eskaera

```json
{
  "context": {
    "messageType": "PERSON_PUSH_TO_ADMIN",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "REQUEST",
    "originPartyId": "DENA-CORE",
    "destinationPartyId": "ADMIN-001",
    "subjectPerson": { "personId": "12345678A" },
    "administration": { "administrationId": "ADMIN-001", "dir3Code": "EA0000001" },
    "interopRouteData": [
      { "denaComponentId": "apiGateway", "timestamp": "2024-06-01T10:00:00Z" }
    ]
  },
  "consentOid": "CONSENT-OID-2024-001",
  "data": {
    "personRef": {
        "id": "12345678A"
    },
    "personHashes": {
        "nameHash": "abcde",
        "surname1Hash": "abcde",
        "surname2Hash": "abcde",
        "allNamesHash": "abcde"
    },
    "createDate": "2024-06-01T10:00:00Z",
    "lastUpdateDate": "2024-06-01T10:00:00Z",
    "syncEvent": "CREATED"
  }
}
```

| Eremua    | Mota                                           | Derrigorrez | Deskribapena |
|-----------|------------------------------------------------|:-----------:|--------------|
| `context` | [Context](../../../semantica-base/index.md) | ✅          | Eskaeraren testuinguru-objektua |
| `data`    | [Data](#data)                                  | ✅          | Eskaeraren payload-a |

## Data

| Eremua           | Mota     | Derrigorrez | Deskribapena |
|------------------|----------|:-----------:|--------------|
| `personRef`      | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Sortutako edo aldatutako pertsonaren erreferentzia |
| `personHashes`   | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Pertsonaren izen eta abizenen hashak identifikazio ezegokigarrirako |
| `createDate`     | `ISO 8601 Date` | ✅ | Sorrera-data |
| `lastUpdateDate` | `ISO 8601 Date` | ❌ | Azken eguneratze-data |
| `syncEvent`      | `String` | ✅ | Gertatu den gertaera. Balio posibleak: <br> `CREATED`: Pertsona berria erregistratua <br> `DELETED`: Pertsona DENA-tik ezabatua <br> `UPDATED`: Pertsonaren datuak eguneratuta <br> `ID_CHANGED`: Pertsonaren identifikatzailea aldatuta |

---

## Erantzun arrakastatsua (HTTP 200)

```json
{
  "context": {
    "messageType": "PERSON_PUSH_TO_ADMIN",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "OK"
}
```

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "context": {
    "messageType": "PERSON_PUSH_TO_ADMIN",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Pertsona ez da sisteman aurkitu" }
}
```

### Egoera-kodeak (`code`)

| Kodea | Deskribapena |
|-------|--------------|
| `OK` | Mezua zuzen prozesatua |
| `CLIENT_ERR` | Bezero-errorea (eskaera gaizki osatua, pertsona ez aurkitua) |
| `SERVER_ERR` | Zerbitzari-errorea (barne-errorea) |
| `QUEUED` | Mezua prozesamendu asinkronorako ilaran jarrita |

---

## Autentifikazioa

Administrazioak OAuth2 eskatzen badu, goiburu hau jasoko du:

```
Authorization: Bearer <access_token>
```

Tokena automatikoki lortzen da client credentials bidez.

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Datuak zuzen itzulita (zerrenda hutsa izan daiteke) |
| `400` | Eskaera gaizki osatua edo parametro baliogabeak |
| `401` | Baimenik gabe (tokena baliogabea edo iraungita) |
| `403` | Debekatuta (baimenik gabe) |
| `404` | Pertsona ez da aurkitu |
| `500` | Barne-errorea |
| `503` | Zerbitzua ez dago eskuragarri |

---

## Administrazioarentzako eskakizunak

1. `POST` endpoint bat esposatu `application/json` onartzen eta itzultzen duena
2. `data.personRef` interpretatu pertsona identifikatzeko
3. DENA-n erregistratutako pertsonen datu-basea eguneratu jasotako informazioarekin
4. HTTP kode estandarrak errespetatu
5. 30 segundotan baino gutxiagoan erantzun
6. Erabili `code: "OK"` erantzun arrakastatsuetan eta `code: "CLIENT_ERR"` edo `code: "SERVER_ERR"` erroreetan

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
