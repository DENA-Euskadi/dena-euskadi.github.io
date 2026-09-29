# :material-web: HTTP Headers

DENAko HTTP dei guztiek testuingurua, segurtasuna eta trazabilitatea ematen duten goiburu estandar eta pertsonalizatuen multzo bat dute.

---

## Request HTTP Headers

| Header | Deskribapena | Adibidea |
|---|---|---|
| `Authorization` | JWT tokena autentifikaziorako (HTTP goiburu estandarra; ez du DENAren traffic-flow-ak kudeatzen) | `Authorization: Bearer {token}` |
| `User-Agent` | Eskaera sortzen duen bezeroaren (nabigatzailea, app-a, liburutegia) datuak. Ikus [UserAgent](./modelo/user-agent.md) | |
| `Content-Type` | Mezuaren eduki mota (normalean `application/json`) | `Content-Type: application/json` |
| `Content-Digest` | Mezuaren **body-aren gainean soilik** kalkulatutako SHA-256 digest-a, Base64-n kodetua | `Content-Digest: SHA-256=:<base64>:` |
| `X-DENA-Data-Digest` | `X-DENA-This-TimeStamp + X-DENA-Message-Correlation-Id + body` kateaketaren gainean kalkulatutako SHA-256 digest-a, Base64-n kodetua | `X-DENA-Data-Digest: SHA-256=:<base64>:` |
| `X-DENA-This-TimeStamp` | Eskaera sortzen duen osagaian eskaera abiarazi zen unea (EPOCH milisegundotan) | `1670374400000` |
| `X-DENA-Origin-TimeStamp` | Hasierako osagaian (adib.: mugikorreko app-a) fluxua abiarazi zen unea (EPOCH milisegundotan). Osagaien artean aldatu gabe mantentzen da | `1670374400000` |
| `X-DENA-Message-Correlation-Id` | Fluxua abiarazi zuen osagaiak sortutako UIDa. Osagaien artean aldatu gabe mantentzen da | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |

!!! warning "Traffic-flow-aren nahitaezko goiburuak"

    Traffic-flow-ak babestutako interoperabilitate-deietan, DENAk `X-DENA-This-TimeStamp`, `X-DENA-Origin-TimeStamp`, `X-DENA-Message-Correlation-Id` eta `X-DENA-Data-Digest` goiburuak **eskatzen** ditu. Bat falta bada, eskaera **HTTP 400**-ekin baztertzen da. `X-DENA-Data-Digest` edo `Content-Digest` falta badira edo jasotako body-arekin bat ez badatoz, **HTTP 401**-ekin baztertzen da.

---

## Response HTTP Headers

| Header | Deskribapena | Adibidea |
|---|---|---|
| `Content-Type` | Erantzunaren eduki mota | `Content-Type: application/json` |
| `Content-Digest` | Erantzunaren body-aren SHA-256 digest-a (Base64) | `Content-Digest: SHA-256=:<base64>:` |
| `X-DENA-Message-Correlation-Id` | Korrelazio-UIDa (eskaeraren oihartzuna) | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Erantzuna sortu zen unea (EPOCH milisegundotan) | `1670374500000` |

---

## Segurtasun-digest-a

`X-DENA-Data-Digest` eta `Content-Digest` goiburuek mezuaren **osotasuna** bermatzeko balio dute. Biek **SHA-256** erabiltzen dute eta emaitzako hash-a **Base64**-n kodetzen da:

- `X-DENA-Data-Digest`: `X-DENA-This-TimeStamp + X-DENA-Message-Correlation-Id + body` kateaketaren SHA-256 hash-a. Body-a bere timestamp-ari eta korrelazio-idari lotzen die.
- `Content-Digest`: **body-aren soilik** SHA-256 hash-a (datuak). EZ ditu goiburuak barne hartzen.

Balioaren formatua bi goiburuetan: `SHA-256=:<base64-hash>:` (algoritmoaren izena, `=:` mugatzailea, Base64 hash-a eta ixteko `:`).

Hartzaileak (traffic-flow sarrerako iragazkiak) bi digest-ak jasotako body-aren gainean birkalkulatzen ditu eta konparatzen ditu; bat ez badatoz, eskaera **HTTP 401**-ekin baztertzen du.

Honek hartzaileari mezua garraioan aldatu ez dela egiaztatzeko aukera ematen dio.

!!! tip "Korrelazioa eta trazabilitatea"
    `X-DENA-Message-Correlation-Id` goiburuak jatorrizko eskaera batetik eratorritako dei guztiak lotzeko aukera ematen du, sistema banatuetan arazketa erraztuz.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
