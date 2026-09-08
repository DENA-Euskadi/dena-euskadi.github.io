# :material-database-arrow-right: Data-Retrieve (Datuak Zerbitzatu)

## Kontzeptua

Data-Retrieve DENAk **zure administrazioari deitzen dion operatiba da, pertsona baten datuak lortzeko**. Zure administrazioak REST endpoint bat erakusten du eta DENAk hori dei egiten du pertsonak bere datu eguneratuak ikusi behar dituenean.

![Data Retrieval Proxy](../adjuntos/imagenes/arquitectura/services-data-retrieval-proxy.png)

### Nola funtzionatzen du?

1. Pertsonak aplikazioa irekitzen du eta DENAk freskatu beharreko datuak daudela detektatzen du
2. DENA-COREk zure endpointa deitzen du zure administrazioaren konektorearen bidez
3. Zure endpointak datu-basea kontsultatzen du eta pertsona horren datuak itzultzen ditu
4. DENA-COREk zehazten du zein datu diren berri/eguneratuak eta aplikaziora bidaltzen ditu

![Data Retrieve Steps](../adjuntos/imagenes/arquitectura/services-data-retrieve-steps.png)

### Funtsezko puntuak

- Zure administrazioak eskatutako datu motaren **datu guztiak** itzultzen ditu pertsona horrentzat
- Datu elementu bakoitzak **`lastChangedAt`** eremu bat izan behar du azken aldaketaren unearekin
- DENA-COREk automatikoki kalkulatzen du datua berria/eguneratua den (administrazioak EZ du adierazi beharrik)
- Datuak **ez dira DENAn gordetzen** — zuzenean aplikaziora pasatzen dira

---

## Inplementazioa

### Endpointa

Zure administrazioak DENAk deituko duen endpoint bat erakusten du:

```
POST /api/retrieveData
```

Endpointak pertsona eta eskatutako datu mota jasotzen ditu, eta datu elementuak itzultzen ditu.

### Adibidea: datu-basearen taula

Demagun "herritarren arretarako hitzorduak" datu mota bat egitura honekin:

| PERSON_ID | AT | DURATION_MIN | SUBJECT_ES | SUBJECT_EU | LAST_UPDATED_AT |
|-----------|-----|------|------------|------------|-----------------|
| 48291038Z | 2026-08-17T14:30 | 30 | Cita renovación | Berritze hitzordua | 2026-08-17T09:14:22Z |
| 48291038Z | 2026-09-04T10:15 | 15 | Consulta estado | Egoera kontsulta | 2026-08-17T18:41:03Z |

### Adibidea: SQL kontsulta

```sql
SELECT PERSON_ID, AT, DURATION_MIN, SUBJECT_ES, SUBJECT_EU,
       COALESCE(LAST_UPDATED_AT, CREATED_AT) AS LAST_CHANGE_AT
  FROM APPOINTMENTS
 WHERE PERSON_ID = ?
```

### Adibidea: DENA modelora bihurtzea

```java
private List<DN00ScheduleItem> _fromDBDataToReturnedDataItems(
        final Collection<DBData> dbData) {
    return dbData.stream()
        .map(item -> {
            DN00ScheduleItem schItem = new DN00ScheduleItem();

            // GARRANTZITSUA: azken aldaketaren unea
            schItem.setLastChangedAt(item.lastChangedAt());
            schItem.setId(DN00ScheduleItemID.forId(item.id()));
            schItem.setAt(item.at());
            schItem.setDurationMinutes(item.durationMins());

            // Hizkuntza anitzeko testuak
            schItem.setSubjectByLanguage(new LanguageTextsMapBacked()
                .add(Language.SPANISH, item.subjectES())
                .add(Language.BASQUE, item.subjectEU()));

            // Xehetasunen URLak zure webean
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

### Adibidea: REST Controller

```java
@RestController
@RequestMapping("/dena/retrieve")
public class AdminAppointmentsController {

    @GetMapping("/citizen_care_appointments/{personId}")
    public ResponseEntity<String> getByPerson(
            @PathVariable("personId") String personIdParam) {

        DN00PersonID personId = DN00PersonID.forId(personIdParam);

        // 1. Datu-basea kontsultatu
        List<DBData> dbData = _retrieveDBDataFor(personId);

        // 2. DENA modelora bihurtu
        List<DN00ScheduleItem> items = _fromDBDataToReturnedDataItems(dbData);

        // 3. Serializatu eta itzuli
        String json = _marshaller.forWriting().toJSON(items);
        return ResponseEntity.ok(json);
    }
}
```

### Adibidea: itzulitako JSONa

Erantzun osoak DENAren interop mezuaren egitura dauka (context + payload). Datu elementuak `payload.dataItems` barruan doaz:

```json
{
  "context": {
    "message": {
      "type": "PERSON_FETCH_DATA",
      "correlationId": "550e8400-e29b-41d4-a716-446655440000",
      "interopRouteData": [
        { "denaComponentId": "ADMIN", "timestamp": "2026-08-19T08:07:56.742Z" }
      ]
    },
    "dataType": { "id": "scheduleItem", "oid": "DTYPE-OID-SCHEDULE" },
    "subjectPerson": { "id": "48291038Z", "oid": "PERSON-OID-001" }
  },
  "payload": {
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

??? note "Mezuaren egitura osoa (oinarrizko semantikaren eremu guztiekin)"

    Erantzun erreal batean, mezuak protokolo eta trazabilitate eremu gehigarriak izan ditzake, DENAk automatikoki kudeatzen dituenak. Zure administrazioak `payload.dataItems` blokea soilik arduratu behar du, baina jarraian egitura osoa erakusten da erreferentzia gisa:

    ```json
    {
      "context": {
        "message": {
          "type": "PERSON_FETCH_DATA",
          "correlationId": "550e8400-e29b-41d4-a716-446655440000",
          "interopRouteData": [
            { "denaComponentId": "DENA_ADMIN_CONNECTOR", "timestamp": "2026-08-19T08:07:55.100Z" },
            { "denaComponentId": "ADMIN", "timestamp": "2026-08-19T08:07:56.742Z" }
          ]
        },
        "originAdmin": { "oid": "ADMIN-OID-001", "id": "MI-ADMIN", "dir3Id": "EA0000001" },
        "userAgent": "MiAdmin/1.0 data-provider/1.0 (soporte@miadmin.eus)",
        "dataType": { "id": "scheduleItem", "oid": "DTYPE-OID-SCHEDULE" },
        "subjectPerson": { "id": "48291038Z", "oid": "PERSON-OID-001" }
      },
      "protocol": {
        "urls": [],
        "timeOut": "30s"
      },
      "payload": {
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

    Bloke bakoitzaren dokumentazio xehatua ikusteko: [:octicons-arrow-right-24: Oinarrizko Semantika](../semantica/semantica-base/index.md)

!!! tip "Xehetasunen URLak"
    Beti sartu URLak non pertsonak xehetasun gehiago ikusi, aldatu edo elementua ezeztatu dezakeen zure webean.

---

## Zehaztapen osoa

Eremuen, baliozkotzeen eta erroreen zehaztapen xehatua ikusteko:

[:octicons-arrow-right-24: Data-Retrieve Semantika](../semantica/data-retrieve/index.md)

---

## Erreferentziazko kodea

Data-Retrieve eta segurtasun goiburuen test eta adibide-kodea duten biltegiak:

| Baliabidea | Biltegia |
|------------|----------|
| HTTP goiburuen definizioak (konstanteak) | [DN00InteropHeaders.java]({{ repos.common_interop_api_blob }}/denaCommonInteropAPIModelClasses/src/main/java/dena/api/common/model/interop/DN00InteropHeaders.java) |
| Espedienteen mock factory-a | [DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceProcedureRecord.java) |
| Jakinarazpenen mock factory-a | [DN99DENATestMockObjFactoryForAdministrativeNotice.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/notification/DN99DENATestMockObjFactoryForAdministrativeNotice.java) |
| Ordainketen mock factory-a | [DN99DENATestMockObjFactoryForOneOffPayment.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/payment/DN99DENATestMockObjFactoryForOneOffPayment.java) |
| Hitzorduen mock factory-a | [DN99DENATestMockObjFactoryForScheduleItem.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/schedule/DN99DENATestMockObjFactoryForScheduleItem.java) |
| Erregistro ofizialen mock factory-a | [DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/register/DN99DENATestMockObjFactoryForAdministrativeOfficialRegisterRecord.java) |
| Oinarrizko eremuak (oid, id, urls) | [DN99DENATestMockObjFactoryForDENADataExchangedObjectBase.java]({{ repos.interop_test_blob }}/denaTestCommonDataClasses/src/main/java/dena/test/common/data/DN99DENATestMockObjFactoryForDENADataExchangedObjectBase.java) |

!!! tip "Erabili mock factory-ak erreferentzia gisa"
    Test hauek guztiz baliozkoak diren adibide-objektuak sortzen dituzte. DENAk espero duen formatu zehatza ikusteko erabil ditzakezu, segurtasun goiburuak eta interop mezuaren egitura barne.

---

**Hurrengoa:** [:octicons-arrow-right-24: Metadata-Sync](./metadata-sync.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
