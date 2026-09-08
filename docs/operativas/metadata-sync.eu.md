# :material-bell-ring: Metadata-Sync (Aldaketak Jakinarazi)

## Kontzeptua

Metadata-Sync zure **administrazioak DENAri datu berriak edo eguneratuak daudela jakinarazten dion** operatiba da pertsona jakin batzuentzat. Hau gabe, DENAk ez daki noiz dauden berrikuntzak eta aplikazioak ezin dio pertsonari jakinarazi datu berriak daudela.

![SRMD High Level](../adjuntos/imagenes/arquitectura/services-srmd-high-level.png)

### Nola funtzionatzen du?

Zure administrazioak **aldizka** (ahal den maiztasunenarekin):

1. Bere DBa kontsultatzen du, azken ziklotik aldatu diren datuak bilatuz
2. Aldaketa bakoitzeko, SRMD erregistro bat sortzen du:
    ```
    {pertsona} | {datu mota} | {admin} | {azken eguneratze unea}
    ```
3. SRMD guztiak DENA-COREra bidaltzen ditu REST bidez

![SRMD Strategies](../adjuntos/imagenes/arquitectura/services-srmd-strategies.png)

### Puntu nagusiak

- **Meta-datuak** soilik bidaltzen dituzu (zer aldatu den eta noiz), EZ datuak beraiek ezta pertsona zehazki nor den ere
- **Datu mota anitzeko** eta **pertsona anitzeko** SRMDak bidal ditzakezu dei bakar batean
- Datu bat berria bada EDO aldatu bada, aldaketa gisa kontatzen da
- Ezabatutako erregistroak ere aldaketa gisa kontatzen dira
- Maiztasun ideala: ahal duzun bezain maiz (minuturo, 5 minuturo, orduro...)

---

## Inplementazioa

### 1. urratsa: Aldaketak kontsultatu zure DBan

DENAk behar duen informazio bakarra hau da: **zein datu mota aldatu da?** eta **noiz izan zen azken aldaketa?**

```sql
SELECT PERSON_ID,
       MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE_AT
  FROM MY_TABLE
 WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= ?
 GROUP BY PERSON_ID;
```

!!! note ""
    `COALESCE(LAST_UPDATED_AT, CREATED_AT)` eguneratze-data erabiltzen du existitzen bada, edo sortze-data inoiz eguneratu ez bada.

### 2. urratsa: DENA eredura bihurtu

```java
private List<DN00SyncMetaDataFromAdminToCOREItem> _toSRMD(
        final Collection<DBPersonLastChange> dbChanges) {
    return dbChanges.stream()
        .map(dbChange -> DN00SyncMetaDataBuilder
            .syncMetaDataFromAdminToCOREBuilder()
            .atAdmin(DN00OrgAdminID.forId("MY_ADMIN"))
            .fromSingleDataOrigin()
            .aboutPerson(DN00PersonID.forId(dbChange.personId()))
            .someDataWasUpdatedAt(dbChange.lastChangedAt())
            .ofType(DN00DataTypeID.forId("citizen_service_appointment"))
            .build())
        .toList();
}
```

### 3. urratsa: Interop mezu batean bildu eta bidali

```java
// Interop mezua sortu
DN00InteropContext ctx = DN00InteropContextBuilder
    .createMessageOfType(DN00InteropMessageType.ADMIN_SYNC_METADATA_REQ)
    .withCorrelationOID(DN00MessageCorrelationOID.supply())
    .fromAdministration(DN00OrgAdminID.forId("MY_ADMIN"))
    .build();

DN00SyncMetaDataFromAdminRequestPayload payload =
    DN00SyncMetaDataFromAdminRequestPayload.from(srmdItems);

DN00SyncMetaDataFromAdminRequestMessage reqMsg =
    new DN00SyncMetaDataFromAdminRequestMessage(ctx, payload);

// JSON formatuan serializatu
String json = marshaller.forWriting().toJSON(reqMsg);

// HTTP POST bidez bidali
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.dena.eus/srmd/"))
    .header("Content-Type", "application/json")
    .header("Accept", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(json))
    .build();
```

### Adibidea: DENAra bidalitako JSONa

```json
[
  {
    "admin": { "id": "EJGV" },
    "aboutPerson": { "id": "40404040H" },
    "someDataWasUpdatedAt": "2026-08-17T15:14:07.036Z",
    "ofType": { "id": "ADMIN_NOTICE" },
    "fromDataOrigin": "DEFAULT"
  },
  {
    "admin": { "id": "EJGV" },
    "aboutPerson": { "id": "12345678A" },
    "someDataWasUpdatedAt": "2026-08-17T15:14:07.032Z",
    "ofType": { "id": "PROCEDURE_RECORD" },
    "fromDataOrigin": "DEFAULT"
  }
]
```

!!! tip "IDak vs OIDak"
    - **Pertsonaren id**-a bere NANa da (adib.: `12345678A`)
    - **Administrazioaren id**-a beti bere IFK/NAN da (adib.: `S4833001C` EJGVrentzat)
    - **Datu motaren id**-a DENA taldearekin adostutako identifikadorea da (adib.: `PROCEDURE_RECORD`)
    - **fromDataOrigin** `DEFAULT` da datu-jatorri bakarra baduzu. Jatorri anitz badituzu, erabili aldez aurretik DENA taldeari jakinarazitako identifikadorea
    
    Ez duzu DENAren barneko OIDak erabili behar. Negozio-IDak nahikoak dira.

!!! note "fromDataOrigin-i buruz"
    Zure administrazioak datu mota bakoitzeko datu-jatorri bakarra badu (ohikoena dena), bidali `"fromDataOrigin": "DEFAULT"` eta ez duzu beste ezertaz kezkatu behar.
    
    Datu mota bererako **jatorri anitz** badituzu (adib.: espediente-kudeatzaile bat baino gehiago), `fromDataOrigin` eremuan jatorri zehatza identifikatu behar da. Identifikadore hau DENA taldearekin adosten da konektorearen konfigurazioan. Aldez aurretik jakinarazten ez baduzu, elementuek huts egingo dute prozesamenduan.

### Adibidea: DENAren erantzuna

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

DENAk itzultzen du zein elementu ondo prozesatu diren (`processedOK`) eta zeintzuk huts egin duten (`processedNOK`) errorearen arrazoiarekin. Bidalketa batek elementu baliodunak eta baliogabeak izan ditzake aldi berean: baliodunak normaltasunez prozesatzen dira eta baliogabeak banaka baztertzen dira gainerakoei eragin gabe.

!!! warning "Errore klasikoa: konfiguratu gabeko data origin"
    `processedNOK`-en akatsik ohikoena DENAren konfigurazioan erregistratu gabeko `fromDataOrigin` bat da. Datu-jatorriari buruzko balidazio-erroreak jasotzen badituzu, egiaztatu DENA taldearekin erabiltzen duzun identifikadorea konektorean konfiguratutakoarekin bat datorrela.

---

## Zehaztapen osoa

Endpoint-aren, ereduaren eta erroreen zehaztapen xehatuetarako:

[:octicons-arrow-right-24: Metadata-Sync Semantika](../semantica/metadata-sync/index.md)

---

**Hurrengoa:** [:octicons-arrow-right-24: Person-Sync](./person-sync.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
