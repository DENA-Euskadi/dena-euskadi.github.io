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
    "message": {
      "type": "PERSON_PUSH_TO_ADMIN",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": [
        { "denaComponentId": "apiGateway", "timestamp": "2024-06-01T10:00:00Z" }
      ]
    },
    "destinationAdmin": { "oid": "6AE83A0C-2202-4666-9857-3334C14663A2", "id": "ADMIN-001", "dir3Id": "EA0000001" },
    "subjectPerson": { "id": "12345678A", "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C" }
  },
  "payload": {
    "personRef": {
        "id": "12345678A",
        "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C"
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
| `context` | [Context](../../../semantica-base/index.md) | ✅          | Eskaeraren testuinguru-objektua. `message.type`, `destinationAdmin` (OrgAdminRef) eta `subjectPerson` (PersonRef) barne hartzen ditu |
| `payload` | [Payload](#payload)                            | ✅          | Eskaeraren payload-a |

## Payload

| Eremua           | Mota     | Derrigorrez | Deskribapena |
|------------------|----------|:-----------:|--------------|
| `personRef`      | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Sortutako edo aldatutako pertsonaren erreferentzia |
| `personHashes`   | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Pertsonaren izen eta abizenen hashak identifikazio ezegokigarrirako |
| `createDate`     | `ISO 8601 Date` | ✅ | Sorrera-data |
| `lastUpdateDate` | `ISO 8601 Date` | ❌ | Azken eguneratze-data |
| `syncEvent`      | `String` | ✅ | Gertatu den gertaera. Balio posibleak: <br> `CREATED`: Pertsona berria erregistratua <br> `DELETED`: Pertsona DENAtik ezabatua <br> `UPDATED`: Pertsonaren datuak eguneratuta <br> `ID_CHANGED`: Pertsonaren identifikatzailea aldatuta |

!!! note "`message.type`-ari buruz"
    `PERSON_PUSH_TO_ADMIN` balioa ez dago oraindik 0.4.16 kodearen `DN00InteropMessageType` enum-ean (definitutako motak ADMIN → DENA-CORE fluxuak dira). DENA-CORE → administrazioa fluxuaren identifikatzaile gisa mantentzen da hemen, dagokion mota erreala gehitu arte.

---

## Erantzun arrakastatsua (HTTP 200)

```json
{
  "context": {
    "message": {
      "type": "PERSON_PUSH_TO_ADMIN",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C" }
  },
  "payload": null,
  "code": "OK"
}
```

## Errore-erantzuna (HTTP 4xx/5xx)

```json
{
  "context": {
    "message": {
      "type": "PERSON_PUSH_TO_ADMIN",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000"
    },
    "subjectPerson": { "id": "12345678A", "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C" }
  },
  "payload": null,
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
2. `payload.personRef` interpretatu pertsona identifikatzeko
3. DENAn erregistratutako pertsonen datu-basea eguneratu jasotako informazioarekin
4. HTTP kode estandarrak errespetatu
5. 30 segundotan baino gutxiagoan erantzun
6. Erabili `code: "OK"` erantzun arrakastatsuetan eta `code: "CLIENT_ERR"` edo `code: "SERVER_ERR"` erroreetan

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
