# Endpoint Person Push To Admin — Administrazioentzako espezifikazioa

Dokumentu honek deskribatzen du **zure administrazioak zer inplementatu behar duen** DENAk proaktiboki bidaltzen dituen pertsonen aldaketa-jakinarazpenak jasotzeko (*Push* mekanismoa).

---

## Nork nori deitzen dio?

Push-en, **DENA-CORE da HTTP bezeroa** eta zure administrazioa zerbitzaria:

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (bezeroa)
    participant Admin as Zure administrazioa (zerbitzaria)

    Note over DENA: Pertsona bat erregistratu / aldatu / ezabatu da
    DENA->>Admin: POST <konfiguratutako-url-a> (aldaketaren JSON gorputza)
    Admin->>Admin: Aldaketa prozesatu (sortu / eguneratu / ezabatu)
    Admin-->>DENA: 200 OK
```

!!! important "Ez dago aurrez definitutako bide finkorik"

    DENAk **ez** du bide zehatzik ezartzen zure endpointerako. Zure administrazioak DENAn bere konektorerako **konfiguratu duen URLan** erakusten du endpointa (edo zuzeneko sarbiderako, garapen-inguruneetan). DENAk URL **horretara** egingo du `POST`.

    DENAk eskatzen duen gauza bakarra da URLak `POST` bat onartzea `Content-Type: application/json`-ekin eta HTTP egoera-kode egokiarekin erantzutea.

---

## Eskaera (Request)

DENAk `POST` bat bidaltzen du, eta bere gorputza `DN00PersonSyncPushToAdminFromCOREToConnectorInternalSide` objektu bat da (`@MarshallType(as="personSyncPushToAdminFromCOREToConnectorInternalSide")`), datu-jatorriaren konfigurazioarekin eta pertsonaren datuak dituen **jakinarazpenarekin**:

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

| Eremua | Mota | Beharrezkoa | Deskribapena |
|--------|------|:-----------:|--------------|
| `dataOriginConfigForDataTypeInAdmin` | `Object` | ✅ | Administrazioko datu-motaren datu-jatorriaren konfigurazioa. Konektoreak erabiltzen duen barne-informazioa da; **zure administrazioak ez du interpretatu behar** |
| `notification` | `DN00PersonSyncPushToAdminNotification` (`@MarshallType(as="personSyncPushToAdminNotification")`) | ✅ | Sinkronizatu beharreko pertsonaren datuak dituen jakinarazpena. **Hau da zure administrazioak prozesatu behar duena** |

### `notification`

| Eremua | Mota | Beharrezkoa | Deskribapena |
|--------|------|:-----------:|--------------|
| `syncData` | `DN00PersonSyncData` (`@MarshallType(as="personSyncData")`) | ✅ | Sinkronizazioaren metadatuak (pertsonaren erreferentzia, hash-ak, datak eta **gertaera**) |
| `person` | `DN00Person` (`@MarshallType(as="person")`) | ✅ | Pertsonaren datu osoak |

### `notification.syncData`

| Eremua | Mota | Beharrezkoa | Deskribapena |
|--------|------|:-----------:|--------------|
| `personRef` | [PersonRef](../../../semantica-base/modelo/person-ref.md) | ✅ | Sortu/aldatu/ezabatu den pertsonaren erreferentzia (`oid` eta/edo `id`). **Hau da zure sisteman pertsona kokatzeko erabili behar duzun gakoa** |
| `personHashes` | [PersonHashes](../../modelo/push/person-hashes.md) | ✅ | Izenaren eta abizenen hash-ak, datuak testu garbian erakutsi gabe identifikazio zalantzagabea egiteko |
| `createDate` | `Instant` (ISO 8601) | ❌ | Pertsona DENAn sortu zeneko data |
| `lastUpdateDate` | `Instant` (ISO 8601) | ❌ | Azken eguneratze-data |
| `syncEvent` | `DN00PersonChangeEvent` | ✅ | **Zer aldaketa gertatu den**. Zure administrazioak egin behar duen ekintza zehazten du ( ikus [Gertaeraka prozesatzea](#gertaeraka-prozesatzea)). Balioak: `CREATED`, `UPDATED`, `DELETED`, `ID_CHANGED` |

### `notification.person`

| Eremua | Mota | Beharrezkoa | Deskribapena |
|--------|------|:-----------:|--------------|
| `oid` | `String` | ✅ | DENAk sortutako pertsonaren identifikatzaile bakarra (egonkorra, ez da aldatzen) |
| `id` | `String` | ✅ | Pertsonaren NANa/AIZ (alda daiteke → ikus `ID_CHANGED` gertaera) |
| `name` | `String` | ✅ | Izena |
| `surname1` | `String` | ✅ | Lehen abizena |
| `surname2` | `String` | ❌ | Bigarren abizena |
| `contactInfo` | `ContactInfo` | ❌ | Kontaktu-datuak |
| `lastChangeEvent` | `DN00PersonChangeEvent` | ❌ | Aplikatutako azken aldaketa mota (DENA-COREk ezartzen du) |

!!! tip "OID vs ID: zein erabili gako gisa"

    Gorde pertsonak beren **`oid`**-aren arabera (DENAren identifikatzaile egonkorra), ez `id`-aren arabera (NAN). NANa alda daiteke (adibidez DNI bihurtzen den AIZ bat) eta kasu horretan `ID_CHANGED` gertaera jasoko duzu. `oid`-aren arabera indexatzen baduzu, NAN-aldaketa horiek eguneratze soil bat dira.

---

## Gertaeraka prozesatzea

`notification.syncData.syncEvent` eremuak esaten dizu **zer ekintza egin**. Hau da zure administrazioak gertaera bakoitzerako espero den inplementazioa:

| Gertaera | Esanahia | Zer egin behar du zure administrazioak |
|----------|----------|----------------------------------------|
| `CREATED` | Pertsona berri bat DENAn erregistratu da | Pertsona zure kopia lokalean altan eman (edo *upsert* jada bazegoen) jasotako datuekin |
| `UPDATED` | Pertsonak oinarrizko datuak aldatu ditu (izena, kontaktua...) | Pertsonaren datuak zure kopia lokalean eguneratu |
| `ID_CHANGED` | Pertsonaren identifikatzailea (NAN/AIZ) aldatu da | Pertsonaren `id` eguneratu (bere `oid`-aren arabera kokatuta, ez baita aldatzen) |
| `DELETED` | Pertsonak bere DENA kontua ezabatu du | Pertsona zure kopia lokaletik ezabatu **eta harekin lotutako datu guztiak ere** |

!!! warning "DELETED gertaera: lotutako datuak ere ezabatu"

    `DELETED` bat jasotzen duzunean, ez da nahikoa pertsonaren erregistroa ezabatzea. **Zure administrazioak pertsona horrekin lotuta zituen datu guztiak ere** kendu behar dituzu (adibidez, sortutako abisu/espedienteak, sinkronizazio-log sarrerak, etab.).

    Pertsona soilik ezabatzen baduzu eta bere datuak uzten badituzu, **datu zurtzak** geratuko dira (jada existitzen ez den pertsona bati erreferentzia egiten dioten errenkadak). Horrek inkoherentziak sortzen ditu eta DENAn jada ez dagoen pertsona baten SRMDak bidaltzera eraman zaitzake.

    Gomendioa: ezabatu lehenik lotutako datuak (pertsonaren identifikatzailearen arabera) eta gero pertsonaren erregistroa.

---

## Erantzuna (Response)

Zure administrazioak emaitza **HTTP egoera-kodearen bidez soilik** adierazten du:

- **`200 OK`** — jakinarazpena behar bezala prozesatu da. **Ez da gorputzik behar.**
- **`4xx`** — eskaerari egotzitako errorea (adib. `400` gorputza baliogabea bada).
- **`5xx`** — zure administrazioaren barne-errorea.

!!! info "DENAk HTTP kodea baino ez du irakurtzen, ez erantzunaren gorputza"

    DENA-COREk (`DN01PersonPushToAdminJobProcessor`) emaitza **HTTP kodetik soilik** interpretatzen du. Erantzunaren gorputza **ez da prozesatzen** (gehienez ere logean erregistratzen da diagnostikorako).

    - Erantzuna arrakastatsua bada (`2xx`), push job-a `SYNCED_OK` egoerara pasatzen da.
    - Ez bada, DENAk **berriro saiatzen da** (gehienezko saiakera-kopururaino) eta, huts egiten jarraitzen badu, job-a `SYNCED_ERROR` / `SYNCED_ERROR_TOO_MANY_ATTEMPTS` egoerara pasatzen da.

    Beraz, funtsezkoa da emaitza errealari **fidela** den HTTP kodea itzultzea: ez itzuli `200` prozesaketak huts egin badu, edo DENAk sinkronizatutzat joko du sinkronizatuta ez dagoen pertsona bat.

Hala ere diagnostikoa errazteko errore-gorputz bat itzuli nahi baduzu (aukerakoa, DENAk ez du interpretatzen), objektu sinple bat erabil dezakezu:

```json
{
  "error": "PERSON_NOT_FOUND",
  "message": "Pertsona ez da aurkitu sisteman"
}
```

---

## Autentifikazioa

Zure administrazioak OAuth2 behar badu, DENAk goiburu hau sartuko du:

```
Authorization: Bearer <access_token>
```

DENAk tokena automatikoki lortzen du *client credentials* bidez. Konfiguratzeko ikus [Autentifikazioa](../../../../autenticacion/core-dena-administracion/index.md) atala.

---

## HTTP kodeak

| Kodea | Esanahia |
|-------|----------|
| `200` | Jakinarazpena behar bezala prozesatu da |
| `400` | Eskaera gaizki osatua edo parametro baliogabeak |
| `401` | Baimenik gabe (token baliogabea edo iraungia) |
| `403` | Debekatua (baimenik ez) |
| `404` | Pertsona ez da aurkitu / ezin da ebatzi |
| `500` | Administrazioaren barne-errorea |
| `503` | Zerbitzua ez dago erabilgarri |

---

## Administrazioarentzako inplementazio-egiaztapena

1. **`POST` endpoint bat erakutsi** DENAn konfiguratu duzun URLan, `application/json` onartuz.
2. **`notification.syncData.syncEvent` irakurri** ekintza erabakitzeko (sortu / eguneratu / ezabatu).
3. **Pertsona `notification.syncData.personRef.oid`-aren arabera kokatu** (identifikatzaile egonkorra).
4. **Aldaketa aplikatu** gertaeraren arabera (ikus [Gertaeraka prozesatzea](#gertaeraka-prozesatzea)), `DELETED`-en lotutako datuak ezabatzea gogoratuz.
5. **Emaitzari fidela den HTTP kodearekin erantzun** (`200` behar bezala prozesatuz gero soilik; `4xx`/`5xx` errorean).
6. **30 segundo baino gutxiagoan erantzun** (bestela DENAk deia hutstzat jotzen du eta berriro saiatuko da).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
