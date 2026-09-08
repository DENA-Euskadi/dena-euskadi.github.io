# :material-check-circle: Status (Erantzuna)

## Deskribapena

`status` egitura **erantzun mezuetan soilik** dago. Prozesamenduaren emaitzari buruzko informazioa barne hartzen du: arrakastatsua izan den, errorea izan den, edo modu asinkronoan prozesatuko den.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `statusCode` | `DENAResponseStatusCode` | :material-check: | Mezuaren prozesamenduko egoera-kodea |
| `statusDetails` | `Object` | :material-close: | Erantzunaren xehetasunak. Mota `statusCode`-aren araberakoa da |

---

## DENAResponseStatusCode

| Balioa | Deskribapena |
|---|---|
| `OK` | Mezua behar bezala prozesatu da |
| `CLIENT_ERR` | Bezeroaren errorea (adib: datu okerrak) |
| `SERVER_ERR` | Zerbitzariaren errorea |
| `QUEUED` | Mezua prozesamendurako ilaran jarri da (asinkronoa) |

---

## Status Details kodearen arabera

| statusCode | statusDetails mota |
|---|---|
| `OK` | *(hutsa)* |
| `CLIENT_ERR` | [DENAClientErrDetails](#denaclienterrdetails) |
| `SERVER_ERR` | [DENAServerErrDetails](#denaservererrdetails) |
| `QUEUED` | [DENAAsyncQueueData](#denaasyncqueuedata) |

---

## DENAClientErrDetails

Errorearen xehetasunak `statusCode = CLIENT_ERR` denean.

| Eremua | Mota | Deskribapena |
|---|---|---|
| `errorCode` | `ID` | Errorearen identifikatzailea. Adib: `ENTITY_NOT_FOUND` |
| `about` | `String` | Errorearekin lotutako entitatearen datuak (adib: personOid/personId) |
| `errorDetails` | `String` | Errorearen xehetasun gehigarriak |

**Adibidea:**

```json
{
  "status": {
    "statusCode": "CLIENT_ERR",
    "statusDetails": {
      "errorCode": "ENTITY_NOT_FOUND",
      "about": "12345678A",
      "errorDetails": "Eskatutako pertsona ez da sisteman existitzen"
    }
  }
}
```

---

## DENAServerErrDetails

Errorearen xehetasunak `statusCode = SERVER_ERR` denean.

| Eremua | Mota | Deskribapena |
|---|---|---|
| `errorCode` | `ID` | Errorearen identifikatzailea. Adib: `UNEXPECTED_ERROR` |
| `errorDetails` | `String` | Errorearen xehetasunak (adib: stack trace) |

**Adibidea:**

```json
{
  "status": {
    "statusCode": "SERVER_ERR",
    "statusDetails": {
      "errorCode": "UNEXPECTED_ERROR",
      "errorDetails": "Connection timeout accessing database"
    }
  }
}
```

---

## DENAAsyncQueueData

Xehetasunak `statusCode = QUEUED` denean (prozesamendua asinkronoa).

| Eremua | Mota | Deskribapena |
|---|---|---|
| `jobToken` | `Token` | Programatutako lanari esleitutako tokena, bere egoera kontsultatzea ahalbidetzen du |
| `jobStatusQueryUrl` | `URL` | Lanaren egoera kontsultatzeko URLa |

**Adibidea:**

```json
{
  "status": {
    "statusCode": "QUEUED",
    "statusDetails": {
      "jobToken": "a3f2c891-4b5d-4e6f-8a9b-1c2d3e4f5a6b",
      "jobStatusQueryUrl": "https://interop.api.dena.eus/queued-jobs/a3f2c891-4b5d-4e6f-8a9b-1c2d3e4f5a6b"
    }
  }
}
```

---

## Lan asinkrono baten egoera kontsultatzea

`jobStatusQueryUrl` kontsultatzerakoan honako hau jasotzen da:

| Eremua | Mota | Deskribapena |
|---|---|---|
| `jobToken` | `Token` | Kontsultatutako lanaren tokena |
| `jobStatus` | `Enum` | Lanaren uneko egoera |

**`jobStatus` balioak:**

| Balioa | Deskribapena |
|---|---|
| `QUEUED` | Ilaran, exekuzioa zain |
| `EXECUTING` | Exekuzio-prozesuan |
| `FINISHED` | Behar bezala amaitua |
| `FAILED` | Errorearekin amaitua |
| `DISCARDED` | Baztertua |

---

## Prozesamendua asinkronoa

!!! info "Fluxu asinkronoa"
    Dei asinkronoetan, emaitza EZ da berehalako erantzun-mezuan sartzen. Token bat itzultzen da eta mezua ilaran jartzen da. Prozesatzea amaitutakoan, bi aukera daude:

    1. **Callback**: zerbitzariak jatorriari jakinarazten dio `protocol` atalean emandako callback URLan
    2. **Polling**: jatorriak aldiro egoera kontsultatzen du `jobStatusQueryUrl` erabiliz

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
