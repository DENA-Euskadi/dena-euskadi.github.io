# :material-transit-connection-variant: DENAProtocol

## Deskribapena

**Mezu-trukeko protokoloaren informazioa** duen objektua. Osagaien arteko komunikazioaren barne-funtzionamendurako beharrezko datuak eskaintzen ditu.

Barne hartzen du:

- URLak (callback, baimen-biltegia, egiaztapena, etab.)
- Timeout-ak
- Hash-ak / Sinadurak
- Tokenak

!!! info "HATEOAS"
    `protocol`-eko URL bildumak [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) (Hypermedia as the Engine of Application State) antzeko funtzionaltasuna ahalbidetzen du.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `urls` | [UrlCollection](../../../arquitectura/tipos-dato-base.md#urls) | :material-close: | ID bidez indexatutako URL bilduma, entitate hartzaileak funtzionaltasuna emateko interpretatu behar duena |
| `timeOutMillis` | `Integer` | :material-close: | Mezuaren jasotzea eta prozesatzea burutzeko gehienezko denbora milisegundotan |

---

## Adibidea

```json
{
  "protocol": {
    "urls": [
      {
        "id": "callback",
        "url": "https://interop.api.dena.eus/callback/async-result"
      },
      {
        "id": "consentVerify",
        "url": "https://interop.api.dena.eus/consent/verify"
      }
    ],
    "timeOutMillis": 30000
  }
}
```

---

## Erabilera-kasuak

| Kasua | `protocol`-en erabilera |
|---|---|
| Dei asinkronoa | Zerbitzariak erantzuna itzuliko duen callback URLa barne hartzen du |
| Baimenen egiaztapena | Administrazioak baimen bat egiazta dezakeen URLa barne hartzen du |
| Denbora-kontrola | Prozesamendurako gehienezko timeout-a ezartzen du |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
