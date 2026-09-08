# :material-web: HTTP Goiburuak

DENAko HTTP dei guztiek goiburu estandar eta pertsonalizatu multzo bat dute, testuingurua, segurtasuna eta trazabilitatea eskaintzeko.

---

## Request HTTP Goiburuak

| Goiburua | Deskribapena | Adibidea |
|---|---|---|
| `Authorization` | Autentifikaziorako JWT tokena | `Authorization: Bearer {token}` |
| `If-Modified-Since` | Cachearen data [RFC 7231](https://datatracker.ietf.org/doc/html/rfc7231) formatuan | |
| `User-Agent` | Eskaera sortzen duen bezeroaren datuak (nabigatzailea, app-a, liburutegia). Ikusi [UserAgent](./modelo/user-agent.md) | |
| `Content-Type` | Mezuaren eduki mota (normalean `application/json`) | `Content-Type: application/json` |
| `Content-Digest` | Mezu osoaren gainean kalkulatutako SHA-256 digest-a | `Content-Digest: sha-256=<digest-value>` |
| `X-DENA-Data-Digest` | Mezuaren datu-zatiaren (body) gainean kalkulatutako SHA-256 digest-a | `X-DENA-Data-Digest: sha-256=<digest-value>` |
| `X-DENA-This-TimeStamp` | Eskaera jatorrizko osagaian hasi zen unea (EPOCH) | `1670374400` |
| `X-DENA-Origin-TimeStamp` | Fluxua hasierako osagaian hasi zen unea (adib: app mugikorra). Osagaien artean aldatu gabe mantentzen da | `1670374400` |
| `X-DENA-Message-Correlation-ID` | Fluxua hasi zuen osagaiak sortutako UID-a. Osagaien artean aldatu gabe mantentzen da | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-App-Version` | Bezero-aplikazioaren bertsioa | `X-DENA-App-Version: 1.3.4` |
| `X-DENA-OS-Version` | Sistema eragilearen bertsioa | `X-DENA-OS-Version: Android 14` |
| `X-DENA-Device-Model` | Gailuaren modeloa | `X-DENA-Device-Model: Pixel 6 Pro` |

---

## Response HTTP Goiburuak

| Goiburua | Deskribapena | Adibidea |
|---|---|---|
| `Content-Type` | Erantzunaren eduki mota | `Content-Type: application/json` |
| `Content-Digest` | Erantzunaren gorputzaren SHA-256 digest-a | `Content-Digest: sha-256=<digest-value>` |
| `X-DENA-Message-Correlation-ID` | Korrelazio UID-a (eskaeraren oihartzuna) | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Erantzuna sortu zen unea (EPOCH) | `1670374500` |

---

## API Bertsio-kontrola

DENAk **custom HTTP Header** bat erabiltzen du APIaren bertsio-kontrolerako:

```
X-DENA-API-VERSION: 1.0
```

!!! info "Kontuan hartutako bertsio-aukerak"

    | Aukera | Adibidea |
    |---|---|
    | URLan bertsioa | `/api/v1.0/files` |
    | Custom HTTP Header (aukeratua) | `X-DENA-API-VERSION: 1.0` |
    | Standard Accept Header | `Accept: application/json;version=1.0` |

---

## Segurtasun digest-ak

`Content-Digest` eta `X-DENA-Data-Digest` goiburuek mezuaren **osotasuna** bermatzen dute:

- `Content-Digest`: **HTTP mezu osoaren** (goiburuak + gorputza) SHA-256 hash-a
- `X-DENA-Data-Digest`: **gorputzaren soilik** (datuak) SHA-256 hash-a

Honek hartzaileak mezua garraioan aldatu ez dela egiaztatzea ahalbidetzen du.

!!! tip "Korrelazioa eta trazabilitatea"
    `X-DENA-Message-Correlation-ID` goiburuak jatorrizko eskaera batetik eratorritako dei guztiak lotzea ahalbidetzen du, sistema banatuetan arazketa erraztuz.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
