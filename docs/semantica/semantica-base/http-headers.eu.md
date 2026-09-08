# :material-web: HTTP Headers

DENAko HTTP dei guztiek testuingurua, segurtasuna eta trazabilitatea ematen duten goiburu estandar eta pertsonalizatuen multzo bat dute.

---

## Request HTTP Headers

| Header | Deskribapena | Adibidea |
|---|---|---|
| `Authorization` | JWT tokena autentifikaziorako | `Authorization: Bearer {token}` |
| `If-Modified-Since` | Cache-datuaren data [RFC 7231](https://datatracker.ietf.org/doc/html/rfc7231) formatuan | |
| `User-Agent` | Eskaera sortzen duen bezeroaren (nabigatzailea, app-a, liburutegia) datuak. Ikus [UserAgent](./modelo/user-agent.md) | |
| `Content-Type` | Mezuaren eduki mota (normalean `application/json`) | `Content-Type: application/json` |
| `Content-Digest` | Mezu osoaren gainean kalkulatutako SHA-256 digest-a | `Content-Digest: sha-256=<digest-value>` |
| `X-DENA-Data-Digest` | Mezuaren datu-zatiaren (body) gainean kalkulatutako SHA-256 digest-a | `X-DENA-Data-Digest: sha-256=<digest-value>` |
| `X-DENA-This-TimeStamp` | Eskaera sortzen duen osagaian eskaera abiarazi zen unea (EPOCH) | `1670374400` |
| `X-DENA-Origin-TimeStamp` | Hasierako osagaian (adib.: mugikorreko app-a) fluxua abiarazi zen unea (EPOCH). Osagaien artean aldatu gabe mantentzen da | `1670374400` |
| `X-DENA-Message-Correlation-Id` | Fluxua abiarazi zuen osagaiak sortutako UIDa. Osagaien artean aldatu gabe mantentzen da | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |

---

## Response HTTP Headers

| Header | Deskribapena | Adibidea |
|---|---|---|
| `Content-Type` | Erantzunaren eduki mota | `Content-Type: application/json` |
| `Content-Digest` | Erantzunaren body-aren SHA-256 digest-a | `Content-Digest: sha-256=<digest-value>` |
| `X-DENA-Message-Correlation-Id` | Korrelazio-UIDa (eskaeraren oihartzuna) | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Erantzuna sortu zen unea (EPOCH) | `1670374500` |

---

## Segurtasun-digest-a

`Content-Digest` eta `X-DENA-Data-Digest` goiburuek mezuaren **osotasuna** bermatzeko balio dute:

- `Content-Digest`: HTTP mezu **osoaren** SHA-256 hash-a (headers + body)
- `X-DENA-Data-Digest`: **body**-aren (datuak) SHA-256 hash-a soilik

Honek hartzaileari mezua garraioan aldatu ez dela egiaztatzeko aukera ematen dio.

!!! tip "Korrelazioa eta trazabilitatea"
    `X-DENA-Message-Correlation-Id` goiburuak jatorrizko eskaera batetik eratorritako dei guztiak lotzeko aukera ematen du, sistema banatuetan arazketa erraztuz.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
