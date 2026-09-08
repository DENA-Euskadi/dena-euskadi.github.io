# :material-check-circle: Status (Erantzuna)

## Deskribapena

Erantzun-mezuek (`DN00InteropResponseMessageBase`) prozesaketaren emaitzari buruzko informazioa jasotzen dute hiru eremuren bidez: `code`, `errorId` eta `details`.

---

## JSON atributuak

| Eremua | Mota | Beharrezkoa | Deskribapena |
|---|---|:---:|---|
| `code` | `DN00InteropResponseStatus` | :material-check: | Mezuaren prozesaketa-egoeraren kodea |
| `errorId` | `DN00InteropResponseStatusCode` | :material-close: | Errore-kode espezifikoa (erroreetan soilik agertzen da) |
| `details` | `DN00InteropResponseStatusDetails` | :material-close: | Erantzunaren xehetasunak (`details` testu-eremu bakarra) |

---

## DN00InteropResponseStatus (`code` eremua)

| Balioa | Deskribapena |
|---|---|
| `OK` | Mezua behar bezala prozesatu da |
| `CLIENT_ERR` | Bezeroaren errorea (adib.: datu okerrak) |
| `SERVER_ERR` | Zerbitzariko errorea |
| `QUEUED` | Mezua prozesaketa asinkronoko ilaran jarri da |

---

## Adibideak

**Erantzun zuzena:**

```json
{
  "code": "OK",
  "errorId": null,
  "details": null
}
```

**Bezeroaren errorea duen erantzuna:**

```json
{
  "code": "CLIENT_ERR",
  "errorId": "ENTITY_NOT_FOUND",
  "details": {
    "details": "La persona solicitada no existe en el sistema"
  }
}
```

**Zerbitzariaren errorea duen erantzuna:**

```json
{
  "code": "SERVER_ERR",
  "errorId": "UNEXPECTED_ERROR",
  "details": {
    "details": "Connection timeout accessing database"
  }
}
```

!!! note "`details`-en egitura"
    `DN00InteropResponseStatusDetails` testu-eremu bakarra (`details`) duen klase bat da. Uneko ereduan ez dago errore-kode bakoitzeko azpimota espezifikorik: errorearen xehetasuna testu libre gisa transmititzen da.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
