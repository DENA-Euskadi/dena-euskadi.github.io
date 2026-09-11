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

Eskaeraren gorputza `DN00PersonSyncPushToAdminFromCOREToConnectorInternalSide` objektu bat da (`@MarshallType(as="personSyncPushToAdminFromCOREToConnectorInternalSide")`), datu-jatorriaren konfigurazioarekin eta pertsonaren datuak dituen **jakinarazpenarekin**:

```json
{
  "dataOriginConfigForDataTypeInAdmin": { "...": "datu-jatorriaren barne-konfigurazioa (konektorearen erabilera)" },
  "notification": {
    "syncData": {
      "personRef": {
        "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C",
        "id": "12345678A"
      },
      "personHashes": {
        "nameHash": "abcde",
        "surname1Hash": "abcde",
        "surname2Hash": "abcde",
        "fullNameHash": "abcde"
      },
      "createDate": "2024-06-01T10:00:00Z",
      "lastUpdateDate": "2024-06-01T10:00:00Z",
      "syncEvent": "CREATED"
    },
    "person": {
      "oid": "9F2C4B7E-1A3D-4E8F-B0C2-5D6E7F8A9B0C",
      "id": "12345678A",
      "name": "Ane",
      "surname1": "Garcia",
      "surname2": "Lopez",
      "contactInfo": { "...": "kontaktu-datuak (ContactInfo)" },
      "lastChangeEvent": "CREATED"
    }
  }
}
```

| Eremua | Mota | Derrigorrez | Deskribapena |
|--------|------|:-----------:|--------------|
| `dataOriginConfigForDataTypeInAdmin` | `Object` | ✅ | Administrazioko datu motarako datu-jatorriaren konfigurazioa. Konektoreak erabiltzen duen barne-informazioa da; administrazioak ez du interpretatu behar |
| `notification` | `DN00PersonSyncPushToAdminNotification` (`@MarshallType(as="personSyncPushToAdminNotification")`) | ✅ | Sinkronizatu beharreko pertsonaren datuak dituen jakinarazpena |

## `notification`

| Eremua | Mota | Derrigorrez | Deskribapena |
|--------|------|:-----------:|--------------|
| `syncData` | `DN00PersonSyncData` (`@MarshallType(as="personSyncData")`) | ✅ | Sinkronizazioaren metadatuak (erreferentzia, hashak, datak, gertaera) |
| `person` | `DN00Person` (`@MarshallType(as="person")`) | ✅ | Pertsonaren datu osoak |

### `notification.syncData`

| Eremua | Mota | Derrigorrez | Deskribapena |
|--------|------|:-----------:|--------------|
| `personRef` | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Sortutako edo aldatutako pertsonaren erreferentzia (`oid`/`id`) |
| `personHashes` | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Izen eta abizenen hashak identifikazio ezegokigarrirako |
| `createDate` | `Instant` (ISO 8601) | ❌ | Sorrera-data |
| `lastUpdateDate` | `Instant` (ISO 8601) | ❌ | Azken eguneratze-data |
| `syncEvent` | `DN00PersonChangeEvent` | ❌ | Sinkronizazioa eragin duen gertaera: `CREATED` (pertsona berria), `DELETED` (pertsona ezabatua), `UPDATED` (datuak eguneratuta), `ID_CHANGED` (identifikatzailea aldatuta) |

### `notification.person`

| Eremua | Mota | Derrigorrez | Deskribapena |
|--------|------|:-----------:|--------------|
| `oid` / `id` | `String` | ✅ | Pertsonaren identifikatzaileak |
| `name` | `String` | ✅ | Izena |
| `surname1` | `String` | ✅ | Lehen abizena |
| `surname2` | `String` | ❌ | Bigarren abizena |
| `contactInfo` | `ContactInfo` | ❌ | Kontaktu-datuak |
| `lastChangeEvent` | `DN00PersonChangeEvent` | ❌ | Aplikatutako azken aldaketa mota (DENA-CORE-k ezartzen du) |

---

## Erantzuna

Administrazioak **HTTP egoera-kodearen** bidez adierazten du prozesamenduaren emaitza:

- **`200 OK`** — jakinarazpena zuzen prozesatu da. Ez da gorputzik behar.
- **`4xx`** — eskaerari egotz dakiokeen errorea (adib. `404` pertsona ezin bada ebatzi, `400` gorputza baliogabea bada).
- **`5xx`** — administrazioaren barne-errorea.

DENA-CORE-k HTTP kodetik interpretatzen du emaitza (ikusi `DN01PersonPushToAdminJobProcessor`): erantzuna arrakastatsua bada, joba `SYNCED_OK` egoerara pasatzen da; bestela, berriz saiatzen da (gehienezko saiakera kopururaino) eta `SYNCED_ERROR` / `SYNCED_ERROR_TOO_MANY_ATTEMPTS` egoerara pasatzen da.

Administrazioak errore-gorputz bat itzultzen badu, mezu deskribatzailea duen objektu sinple bat gomendatzen da, adibidez:

```json
{
  "error": "PERSON_NOT_FOUND",
  "message": "Pertsona ez da sisteman aurkitu"
}
```

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
2. `notification.syncData.personRef` (eta `notification.person`) interpretatu pertsona identifikatzeko
3. DENAn erregistratutako pertsonen datu-basea eguneratu jasotako informazioarekin
4. HTTP kode estandarrak errespetatu (`200` zuzen prozesatzen bada; `4xx`/`5xx` errorean)
5. 30 segundotan baino gutxiagoan erantzun

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
