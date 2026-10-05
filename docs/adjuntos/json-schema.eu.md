# :material-code-json: REST mezuen JSON Schema

**JSON Schema (draft 2020-12)** eskema, DENAren elkarreragingarritasun REST mezu guztien eta trukatzen dituzten datu-ereduen egitura deskribatzen duena (espedienteak, jakinarazpenak, ordainketak, pertsonak, hitzorduak, etab.).

!!! info "Zertarako balio du?"

    Eskema hau **DENA CORE-ren benetako ereduetatik sortutako** erreferentzia bat da. Honetarako erabil dezakezu:

    - Zure administrazioak bidaltzen edo jasotzen dituen mezuak (request/response) **balioztatzeko** integratu aurretik.
    - Zure hizkuntzan **motak / bezeroak sortzeko** (TypeScript, Java, Python…) eskematik abiatuta.
    - Onartutako eremuak, motak eta enum balioak begi-kolpe batez **kontsultatzeko**.

---

## Deskarga

| Baliabidea | Deskribapena | Deskarga |
|------------|--------------|:--------:|
| **DENA REST Messages — JSON Schema** | REST mezu guztien eskema (data-retrieve, metadata-sync, person-sync, person fetch/head/search) eta trukatutako datu-ereduena. | [:material-download: JSON Schema](./json-schema/dena-rest-messages.schema.json) |

---

## Barne hartutako mezuak

Eskemak, besteak beste, goi-mailako mezu hauek deskribatzen ditu:

| Operatiba | Mezua (request / response) |
|-----------|----------------------------|
| **Data-Retrieve** | `DN00COREToConnectorDataRetrieveRequestMessage` / `DN00COREToConnectorDataRetrieveResponseMessage` |
| **Metadata-Sync (SRMD)** | `DN00SyncMetaDataFromAdminRequestMessage` / `DN00SyncMetaDataFromAdminResponseMessage` |
| **Person-Sync (push)** | `DN00PersonInteropPushToAdminNotificationMessage` |
| **Person-Sync (pull bespoke)** | `DN00PersonInteropPullFromAdminBespokeJobCreate…` / `…BespokeJobGet…` / `…BespokeExportAssetFetch…` |
| **Person-Sync (pull pregen)** | `DN00PersonInteropPullFromAdminPreGenJobGet…` / `…PreGenExportAssetFetch…` |
| **Pertsona (fetch / head / search)** | `DN00PersonFetchInterop…` / `DN00PersonHeadInterop…` / `DN00PersonInteropSearch…` |

Trukatzen den datu-objektu bakoitzak **mota-bereizgarri** bat darama (`type` eremua), adibidez `administrativeServiceProcedureRecord`, `administrativeNotice`, `payment`, `directDebitPayment`, `personData` edo `scheduleItem`.

---

!!! tip "Semantika zehatza"

    Mezu eta eredu bakoitzaren eremuz eremuko deskribapen funtzionala [Semantika](../semantica/index.md) atalean dago. Eskema hau dokumentazio horren osagarri **formala eta balioztagarria** da.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
