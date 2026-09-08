# :material-file-document: DENA Arkitektura - Dokumentazio Osoa

> Dokumentu hau `DENA-Architecture.docx` eta `DENA-CORE-Services_for_admins.docx` fitxategietatik ateratako informazioa dauka erreferentziarako.

---

## 1. DENA Ikuspegi Orokorra

### 1.1 DENA Premisak

DENA **pertsonen** eta **administrazioen** arteko interoperabilitate sistema da, administrazioen artekoa ez.

**Oinarrizko printzipioak:**

- DENA-APP datuak zuzenean berreskuratzen ditu administraziotik DENA-COREk PROXY gisa jarduten duela
- **EZ da daturik gordetzen DENA-COREn**
- Pertsona batek administrazio batetik datuak berreskuratzen duen bakoitzean, pertsonak emandako baimena derrigorrezkoa da
- DENA-APP berreskuratutako datuak gorde ditu erabiltzailearen gailuko cache lokal batean
- DENA-APPk jakin behar du:
  - **Daten jatorria**: zein administraziok daukan informazio pertsona bati buruz
  - **Jatorrizko datuen aldaketak**: cache lokalak eguneraketa behar duen jakiteko aukera ematen du

### 1.2 DENA Kontzeptu Gakoak

| Kontzeptua | Deskribapena |
|------------|--------------|
| **[person]** | DENAn hasten den pertsona (DENA kontua sortzen du) |
| **[admins]** | DENA ekosistemi datuak eskaintzen dizkioten entitate publikoak |
| **DENA-APP** | Pertsonak beren datuetara sartzeko erabiltzen duten app mugikorra edo web |
| **DENA-CORE** | DENA-APPrako eta interoperabilitaterako zerbitzuak eskaintzen dituen sistema |
| **[data origin]** | Pertsona baten datuen iturria administrazio batean |
| **SRMD** | Sync And Retrieve Meta-Data: datuen aldaketei buruzko metadatuak |
| **[admin connector]** | DENA-COREren osagaia administrazioekin aritzeko |
| **[data provider]** | Administrazioak datuak eskaintzeko eskaintzen duen zerbitzua |

---

## 2. Zerbitzu COREak Administrazioentzat

DENA-COREk hurrengo zerbitzuak eskaintzen ditu administrazioentzat:

### 2.1 SRMD Sync: Aldaketei buruzko informazioa bidaltzea

Mekanismoa administrazioei datuen aldaketen informazioa DENA-COREri bidaltzeko.

![SRMD Sync Overview](../../adjuntos/imagenes/image1.png)

**Fluxu nagusia:**

1. Administrazioak bere negozio-BD kontsultatzen du azken ziklotik aldatu diren elementuak bilatzeko
2. Aldatu den elementu bakoitzeko, SRMD egitura sortzen du:
   ```
   {person} | {data type} | {admin} | {last update instant}
   ```
3. Administrazioak SRMD GUZTIAK bidaltzen dizkio DENA-COREri REST zerbitzu baten bidez

**Inplementazio estrategiak:**

- **Data provider bakarra** administrazio bateko data type bakar batentzat
- **Bildutako data provider-ek** jatorri anitzeko datuak konbinatuz
- **Data provider anitz** sistema desberdinetako jatorri anitzentzat

**Administrazio batek jatorri anitzeko data origin dituenean data type bererako, SRMDk hau barne hartzen du:**

```
{person} | {admin} | {data type} | {data origin instance} | {last update instant}
```

### 2.2 Data Retrieval: Zerbitzua eskaintzea

DENA-COREk intermediario/proxy gisa jarduten du DENA-APPk administrazioetatik datuak berreskuratzeko.

![Data Retrieval Overview](../../adjuntos/imagenes/image5.png)

**Berreskuratze fluxua:**

1. Bezeroak DENA-CORE deitzen du datuak eskatuz (data type, admin, person)
2. DENA-COREk mezua modelo-objektuetan itzultzen du
3. DENA-COREk erabili beharreko konektorea zehazten du
4. Konektoreak administrazioaren data provider-ari deitzen dio
5. Datuak formatu estandarrera itzultzen dira behar izanez gero
6. DENA-COREk "aldatu da" egoera zehazten du
7. Erantzuna bezerora iristen da

**Administrazioak aukera du:**
- Data providerra DENAren semantika estandarrak erabiliz inplementatzea
- Semantika pertsonalizatuak inplementatzea (SOAP, X-509, formatu propioa)

---

## 3. Datu Truke Mekanismoa

### 3.1 SYNC (Aldaketei buruzko meta-datuak)

DENA-APP cache lokal bat erabiltzen du non gordetzen diren berreskuratutako datuak. Eguneratuta mantentzeko:

1. Administrazioek aldizka bidaltzen dizkiote SRMDak DENA-COREri datu moten aldaketaekin
2. DENA-APPk logeatutako pertsonaren SRMDak eskuratzen ditu eta bere DB lokalean gordetzen ditu
3. SRMD lokala DENA-CORErenarekin alderatuz, DENA-APPk jakingo du zein datu aldatu diren

**SRMD Egitura:**
```
[person] | [admin] | [data type] | [data origin instance] | [last update instant]
```

### 3.2 RETRIEVE (Datuen berreskuratzea)

Aldaketa bat detektatzen denean, DENA-APPk datu osoak berreskuratzen ditu:

1. DENA-APPk DENA-COREn zerbitzu REST bat deitzen du
2. DENA-COREk mezua modelo-objektuetan itzultzen du
3. DENA-COREk erabili beharreko konektorea zehazten du
4. Konektoreak administrazioaren datu-hornitzaileari deitzen dio
5. Datuak formatu estandarrera itzultzen dira behar izanez gero
6. DENA-COREk elementu bakoitearen "aldatu da" egoera zehazten du
7. Erantzuna DENA-APPeraira iristen da

### 3.3 COLD-START Arazoa

Pertsona bat DENAn hasten denean, ez dago SRMDrik administrazioek EZ baitute SRMDak bidaltzen erroldatu gabeko pertsonentzat.

**Soluzioa:**
DENA-COREk SRMD sarrerak txertatzen ditu administrazio giltzek bidali balituz bezala (EJGV, DFB, DFG, DFA, Bilbo, Gasteiz, Donostia).

---

## 4. Person-Sync Xehetasuna

### 4.1 Pertsona Bakoitzeko Gordetako Datuak

DENAn erroldatutako pertsona bakoitzak dauka:

- **Person OID**: DENAk sortutako identifikazio bakarra
- **Person ID**: NAFa
- **Izen Osoa**: izena, abizena1, abizena2
- **Kontaktu Datuak**: helbidea, telefonoa, emaila...
- **Gogoko Gaiak**: zerbitzu proaktiboetarako edo UI pertsonalizatzeko

### 4.2 Sinkronizazio Mekanismoak

#### DENA-PUSH: DENAk administratzioari jakinarazten dio

DENAk mezua bidaltzen du:
- Perston berri bat erregistratzen denean
- Pertsonak bere kontua ezabatzen duenean
- Pertsonak oinarrizko datuak aldatzen dituenean (izena, kontaktua...)

![DENA Push Overview](../../adjuntos/imagenes/image7.png)

#### ADMIN-PULL: Administrazioak kontsultatzen du

**Modalitateak:**

| Modalitatea | Deskribapena |
|--------------|--------------|
| **Lineakoa** | Kontsulta zerbitzu REST denbora errealean |
| **Lineaz kanpoko (aurregeneratua)** | Fitxategi periodiko aurregeneratuak |
| **Lineaz kanpoko (bespoke)** | Fitxategi pertsonalizatuak eskatuta sortuak |

**Lineako Pull Adibidea:**

```
POST /persons/search
{
  "orgAdminRef": { "id": "admin1" },
  "personQuery": {
    "personIds": ["40404040H", "12345678Z"]
  }
}
```

**Lineaz kanpoko Pull - Aurregeneratua Adibidea:**

```
POST /pre-generated
{
  "jobType": "ALL_PERSONS",
  "exportType": "DATA",
  "fileFormat": "CSV",
  "hourOfDay": "20"
}
```

**Lineaz kanpoko Pull - Bespoke Adibidea:**

```
POST /bespokes
{
  "orgAdminRef": { "id": "admin1" },
  "exportSpec": {
    "personExportSpec": "data",
    "lastUpdateRange": "Instant:[2026-08-24T21:19:41.314878600Z..)",
    "exportContentSpec": {
      "exportDefaultContactData": true,
      "exportOtheContactData": true,
      "exportFinData": true
    },
    "exportFileFormat": "CSV"
  }
}
```

!!! warning "Desberdintasun garrantzitsua"

    Person-Sync ≠ Kontaktu Datuak datu mota gisa:
    - **Person-Sync**: Mekanismoa administratzioek DENAko pertsona kontuen datuak eskuratzeko
    - **Kontaktu Datuak**: Beste datu mota interoperagarri bat pertsonei beren kontaktu datuak administrazioetan ikusteko aukera ematen dieena

---

## 5. Zerbitzu Arkitektura

### 5.1 Ikuspegi Orokorra

Arkitekturaren bloke nagusiak hauek dira:

- **Client Device UI**: DENA-APP
- **DENA-CORE**:
  - Konfigurazio Modulua (antolaketa-egitura, interop konfigurazioa)
  - Pertsonen Modulua (DENA kontuak, sinkronizazioa)
  - Sync and Retrieve Modulua (SRMD)
  - Baimen Modulua
  - Segurtasun Modulua
- **Admin Konektoreak**: Semantika estandarraren eta zehatzaren arteko itzulpena

![Architecture Overview](../../adjuntos/imagenes/image2.png)

### 5.2 Konektoreak

**konektore** bat DENAren barruan erabiltzen diren semantikak itzultzen ditu administrazioaren datu-hornitzailearen semantika zehatzera.

![Connectors Architecture](../../adjuntos/imagenes/image6.png)

**Konektorearen bi aldeak:**
1. **Barruko aldea**: DENAren semantika estandarrak erabiltzen ditu (garraioa, segurtasuna, formatua)
2. **Kanpoko aldea**: urruneko hornitzailearekin nola elkarreragin jakiten du (URL, header-ak, autentikazioa, datu-formatua)

Konektoreak DENA-COREtik independenteak diren moduluak dira, sistema erresilenteagoa egiteko.

---

## 6. Oinarrizko Datu Motak

### 6.1 Motak Fundamentalezkoak

| Mota | Deskribapena | Adibidea |
|------|--------------|----------|
| **Boolean** | true/false balioak | `true` |
| **Numbers** | Osokoak eta hamartarrak | `42`, `3.14` |
| **Enums** | Multzo definituko balioak | `HIZKUNTA: ESPANIOLA \| EUSKARA \| INGELES` |
| **String** | Testu-kateak | `"Euskadi"` |
| **LanguageTexts** | Testu eleanitzak | `{ "es": "Hola", "eu": "Kaixo" }` |
| **DateTime** | Denbora-instantak | `2024-01-15T10:30:00Z` |
| **UID/OID** | Identifikazio bakarrak | `urn:uuid:12345678-1234...` |
| **ID** | Esanahi komertzialeko identifikazioak | NAF, IBAN, espediente-zenbakia |
| **Money** | Diru-kopuruak | `{ "base": "ZENTIMOAK", "currency": "EUR", "amount": 249333 }` |
| **Hash** | SHA-256 digesta | `2251f5ac60e4fc24a62914bf92a9adf3af1abbf72d649eceec9ed7c3a2e29b8f` |
| **UserAgent** | Bezeroaren informazioa | `DenaApp/1.0 (Android 13; Pixel 6 Pro)...` |

### 6.2 Egitura Konplexuak

- **Urls**: URL osagaiak parseatuta
- **WebLink**: Web esteka testu eleaniztuekin
- **WebLinkCollection**: WebLink bilduma
- **Ranges**: Balio-barrutiak
- **Token**: Segurtasun tokenak

---

## 7. Datu Trukea: JSON Adibideak

### 7.1 SRMD Adminetik DENA-COREra bidalia

```json
{
  "admin": { "id": "EJGV" },
  "aboutPerson": { "id": "40404040H" },
  "someDataWasUpdatedAt": "2026-08-17T15:14:07.0369127Z",
  "ofType": { "id": "ADMIN_NOTICE" },
  "fromDataOrigin": "DEFAULT"
}
```

### 7.2 DENA-COREn erantzuna SRMDra

```json
{
  "transactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
  "receivedItemsCount": 3,
  "processedOK": [
    {
      "oid": "4F4EF685-BA83-4BAB-80CD-6B01BBE7939C",
      "lastTransactionOid": "20863FFE-0BEB-4079-BFA1-A1EAEEEB58FF",
      "admin": { "id": "EJGV" },
      "dataType": { "id": "ADMIN_NOTICE" },
      "dataOriginInstanceId": "DEFAULT",
      "person": { "id": "40404040H" },
      "dataLastUpdatedAt": "2026-08-17T15:14:07.0324014Z"
    }
  ],
  "processedNOK": []
}
```

---

## 8. Segurtasuna

### 8.1 Pertsonen Autentifikazioa

- Saio hasiera **GILTZA OAUTH tokena**rekin
- Ondoren **Passkeys** erabiltza GILTZA berriro eskatu behar izan ez dadin

### 8.2 DENA-APPren Autentifikazioa DENA-COREkin

- **DENA-IdP**ren OAUTH tokena

### 8.3 Administrazioaren Autentifikazioa DENA-COREkin

- **DENA-IdP**ren OAUTH tokena

### 8.4 DENA-COREren Autentifikazioa Administrazioarekin

- Administrazio helburuak nahiago duen autentifikazio sistema
- OAUTH, X-509 ziurtagiria, erabiltzailea/password...

---

## 9. DENA Konfigurazioa

### 9.1 Etiketa Sistema

Administrazioen antolaketa-egitura definitzen du.

### 9.2 Interoperabilitate Konfigurazioa

| Elementua | Deskribapena |
|-----------|--------------|
| **Data Type** | Administrazioek eskaintzen dituzten datu motak |
| **Data Origin** | Konektoreek erabiltzen duten konfigurazioa |
| **AppVersion** | Aplikazioaren bertsio historikoa |

---

## 10. Arkitektura Irudiak

Hurrengo irudiak erabilgarri daude `docs/adjuntos/imagenes/` karpetan:

| Fitxategia | Deskribapena |
|------------|--------------|
| `image1.png` | SRMD kontzeptuen diagrama |
| `image2.png` | Konektore arkitektura orokorra |
| `image3.svg` | Sinkronizazio fluxua |
| `image5.png` | Data Retrieval ikuspegi orokorra |
| `image6.svg` | Konektore arkitektura xehetua |
| `image7.png` | DENA Push ikuspegi orokorra |
| `image9.png` | Datu truke fluxua |
| `image10.png` | Arkitektura maila altuko ikuspegia |
| `image11.png` | Inplementazio adibidea |
| `image12.png` | Trukatutako datuak |

---

**Azken eguneraketa:** DENA-Architecture.docx eta DENA-CORE-Services_for_admins.docx fitxategietatik aterata

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>