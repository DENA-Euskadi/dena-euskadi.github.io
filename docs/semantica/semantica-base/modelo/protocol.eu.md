# :material-transit-connection-variant: DENAProtocol

## Deskribapena

**Mezu-trukeko protokoloaren informazioa** duen objektua. Osagaien arteko komunikazioaren barne-funtzionamendurako beharrezko datuak eskaintzeko erabiltzen da.

Barne hartzen du:

- URLak (callback, baimen-biltegia, egiaztapena, etab.)
- Timeout-ak
- Hash-ak / Sinadurak
- Tokenak

!!! info "HATEOAS"
    `protocol`-eko URL bildumak [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) (Hypermedia as the Engine of Application State) antzeko funtzionaltasuna eskaintzen du.

---

## JSON Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `urls` | [UrlCollection](../../../arquitectura/tipos-dato-base.md#urls) | :material-close: | ID bidez indexatutako URL bilduma, entitate hartzaileak funtzionaltasuna emateko interpretatu behar duena |
| `timeOut` | `TimeLapse` | :material-close: | Mezua jaso zenetik prozesatzea burutzeko denbora-muga (`TimeLapse` formatua, adib. `"30s"`, `"1m"`) |

Klasea: `DN00InteropProtocol` (`@MarshallType(as="protocol")`).

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
    "timeOut": "30s"
  }
}
```

---

## Erabilera-kasuak

| Kasua | `protocol`-en erabilera |
|---|---|
| Dei asinkronoa | Zerbitzariak erantzuna itzuliko duen callback URLa barne hartzen da |
| Baimenen egiaztapena | Administrazioak baimen bat egiazta dezakeen URLa barne hartzen da |
| Denbora-kontrola | Prozesamendurako gehienezko timeout-a ezartzen da |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
