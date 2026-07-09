# Erroreak eta Troubleshooting

## Deskribapena

DATA-RETRIEVE endpoint-a inplementatzean ohiko erroreen gida eta nola konpondu.

---

## Eskaerako erroreak (administrazioak jasotakoak)

### 400 — Eskaera gaizki osatua

| Kausa | Konponbidea |
|-------|-------------|
| Body-ko JSON baliogabea | Egiaztatu body-a JSON balioduna dela prozesatu aurretik |
| `context.subjectPerson.personId` falta da | Derrigorrezko eremua — egiaztatu jasotzen dela |
| `context.dataType.dataTypeId` falta da | Derrigorrezko eremua — egiaztatu jasotzen dela |
| `dataTypeId` ezagutzen ez den balioarekin | Onartu soilik: `RECORDS`, `NOTICES`, `REGISTER`, `PAYMENTS`, `SCHEDULE`, `PERSON_DATA` |

### 401 — Baimenik gabe

| Kausa | Konponbidea |
|-------|-------------|
| OAuth2 tokena falta da | Egiaztatu `Authorization: Bearer <token>` goiburua dagoela |
| Tokena iraungita | DENAk tokenak automatikoki berritzen ditu; irauten badu, berrikusi client credentials konfigurazioa |
| Tokena baliogabea | Egiaztatu endpoint-ak DENAn konfiguratutako baimena-zerbitzari berdinaren aurka balioztatzen duela |

### 404 — Pertsona ez da aurkitu

| Kausa | Konponbidea |
|-------|-------------|
| `personId` ez dago administrazioaren sisteman | Itzuli HTTP 404 errore-body estandarrarekin |
| `personId` formatua ez da ezagutzen | Onartu NAN (8 zifra + letra), AIZ (X/Y/Z + 7 zifra + letra) eta IFZ |

---

## Erantzuneko erroreak (administrazioak itzulitakoak)

### Errore-egitura estandarra

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
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

| Eremua | Deskribapena |
|--------|--------------|
| `code` | `CLIENT_ERR` (bezero-errorea) edo `SERVER_ERR` (zerbitzari-errorea) |
| `errorId` | Administrazioak definitutako errore-kode espezifikoa (aukerakoa) |
| `details.details` | Errorearen mezu deskribatzailea |

### Egoera-kodeak (`code`)

| Kodea | Noiz erabili |
|-------|--------------|
| `OK` | Erantzun arrakastatsua (zerrenda hutsarekin ere) |
| `CLIENT_ERR` | Eskaera gaizki osatua, pertsona ez aurkitua, parametro baliogabeak |
| `SERVER_ERR` | Administrazioaren zerbitzariaren barne-errorea |
| `QUEUED` | Mezua prozesamendu asinkronorako ilaran jarri da |

### `dataItems`-eko formatu-errore ohikoak

| Errorea | Sintoma DENAn | Konponbidea |
|---------|----------------|-------------|
| `type` eremua falta da objektu batean | DENAk ezin du objektua deserializatu | Beti sartu `type` eremua balio zuzenarekin |
| Data formatu okerrean | Analisi-errorea | Erabili ISO 8601: `2024-03-15T10:30:00Z` |
| `LocalDate` data ordurekin | Analisi-errorea | Erabili soilik `YYYY-MM-DD` ordurik gabeko data-eremuetarako |
| `LanguageTexts` hizkuntza-gako baliogabearekin | Testua ez da erakusten | Erabili `SPANISH`, `BASQUE` edo `ENGLISH` (maiuskulaz) |
| `amountInEuro` string gisa | Mota-errorea | Bidali zenbaki gisa: `45.50`, ez `"45.50"` |
| `urls` array ordez objektu gisa | Deserializazio-errorea | `urls` beti array bat da, elementu bakarra badu ere |
| `state` string gisa espedientean | Mota-errorea | Espedientean eta erregistroan, `state` objektu bat da `stateCode` eta `description`-rekin |
| `state` objektu gisa jakinarazpenean | Mota-errorea | Jakinarazpenean, `state` zuzenean string bat da (egoera-kodea) |

---

## Konektibitate-arazoak

| Arazoa | Diagnostikoa | Konponbidea |
|--------|--------------|-------------|
| DENAk ezin du endpoint-era konektatu | Timeout edo connection refused | Egiaztatu endpoint-a DENAren saretik eskuragarri dagoela |
| SSL ziurtagiri baliogabea | TLS handshake errorea | Erabili CA ezagun batek emandako ziurtagiri balioduna |
| Erantzuna oso motela | Timeout (30s lehenespenez) | Optimizatu kontsultak; kontuan hartu paginazioa datu asko badaude |
| Erantzuna oso handia | Memoria-errorea | Mugatu `dataItems` gehieneko arrazoizko batera (< 1000 elementu) |

---

## Datu-arazoak

| Arazoa | Sintoma | Konponbidea |
|--------|---------|-------------|
| Espedientea zerbitzurik eta prozedurarik gabe | Objektua balioztatze-erroreak baztertua | `service` eta `procedure` derrigorrez dira espedienteetan |
| Ordainketa `forStatus: COMPLETED`-rekin baina `paidAt` gabe | Datu-inkoherentzia | Ordainketa osatuta badago, sartu `paymentDates.paidAt` |
| Jakinarazpena `readedAt`-rekin baina `PENDING` egoerarekin | Datu-inkoherentzia | Irakurketa-data badu, egoera `ACKNOWLEDGED` edo `REJECTED` izan behar da |
| Hitzordua `durationMinutes` negatiboarekin | Balioztatze-errorea | Erabili 0 mugarrietarako, balio positiboak iraupena duten hitzorduetarako |
| Domiziliazioa `directDebitData` gabe | Objektu osatugabea | `directDebitData` blokea derrigorrez da domiziliazioetan |

---

## Inplementazio-zerrenda

DENArekin konektatu aurretik, egiaztatu:

- [ ] Endpoint-ak `POST` onartzen du `Content-Type: application/json`-rekin
- [ ] Endpoint-ak `Content-Type: application/json` itzultzen du
- [ ] `context.subjectPerson.personId` zuzen interpretatzen da
- [ ] `context.dataType.dataTypeId` zuzen interpretatzen da
- [ ] `dataItems`-eko objektu guztiek `type` eremua dute
- [ ] Hizkuntz anitzeko testuek gutxienez `SPANISH` eta `BASQUE` dituzte
- [ ] Datak ISO 8601 formatuan daude
- [ ] Zenbatekoak zenbakiak dira (ez stringak)
- [ ] URLak HTTPS baliodun dira
- [ ] `{ "data": { "dataItems": [] }, "code": "OK" }` itzultzen da daturik ez dagoenean
- [ ] HTTP 200 itzultzen da zerrenda hutsa denean ere
- [ ] HTTP errore-kodeak zuzen erabiltzen dira (400, 401, 404, 500)
- [ ] `code: "OK"` erabiltzen da arrakastaren kasuan eta `code: "CLIENT_ERR"` / `code: "SERVER_ERR"` erroreen kasuan
- [ ] Endpoint-ak 30 segundotan baino gutxiagoan erantzuten du

---

## Kontaktua eta laguntza

Gida hau berrikusi ondoren erroreak irauten badu, jarri harremanetan DENA taldearekin honako hau emanez:

1. Eskaeraren `messageCorrelationId`
2. Itzulitako HTTP kodea
3. Erantzunaren body-a (aplikatzen bada)
4. Zerbitzariaren logak timestamp-arekin

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
