# :material-history: Changelog

DENA dokumentazioaren bertsio-historia.

---

## v0.4.0 <small>— 2026-08-24</small> { #v040 }

!!! success "Unekoa"

**Semantika BASE v2.0 dokumentuaren integrazioa:**

- :material-plus: HTTP Goiburuak: request/response goiburuak, API bertsio-kontrola, segurtasun digest-ak
- :material-plus: Baimenak: printzipioak, arau-esparrua (DBEO), bizi-zikloa, formatua eta APIa
- :material-plus: DenaObjectRef: oinarrizko mota OID, timestamp eta URLarekin
- :material-plus: DENAProtocol: callback URLak, timeout-ak, HATEOAS
- :material-plus: DENAConsent: Data-Retrieve mezuetako oinarri gaitzailearen erreferentzia
- :material-plus: Status: erantzun-kodeak, DENAClientErrDetails, DENAServerErrDetails, DENAAsyncQueueData
- :material-plus: Mezu Motak: DenaFlowDirection, DenaMessageType, DenaInteropRouteDataItem, DenaPersonAndConsentGiven
- :material-plus: UserAgent: formatua jatorriaren arabera (app mugikorra, web, CORE, administrazioa)
- :material-plus: Sync Motak: DenaInteroperableDataTypeSync, DenaPersonDataSync, DenaPersonMetaDataSyncItem
- :material-refresh: OrgAdminRef hedatua orgId, officialId, objectOid, timestamp, url, egitura organizatiboarekin
- :material-refresh: PersonRef hedatua personId, objectOid, timestamp, url-rekin
- :material-refresh: tipos-dato-base: Hash (SHA-256) eta UserAgent gehituta
- :material-refresh: semantica-base/index: mezuaren egitura osoa (context, protocol, consent, status, data)
- :material-refresh: mkdocs.yml: nabigazioa orrialde berriekin eguneratuta
- :material-translate: EN eta EU itzulpenak orrialde berri eta eguneratu guztietarako (76/76)

---

## v0.3.38 <small>— 2026-07-04</small> { #v0338 }

**Dokumentazio-hobekuntzak:**

- :material-palette: CSS fitxategi zenbakituetan (`01`–06) berrantolatuta erantzukizunaren arabera
- :material-refresh: `extra.css` fitxategi zenbakituetara migratuta; legacy fitxategia hustu
- :material-image: Administrazioen logoak footer-ean gehituta (Ayto. Bilbao, Donosti, Vitoria, DFA, DFB, DFG, EUDEL)
- :material-format-size: Footer-eko logoen tamaina 60px-era handitu
- :material-bug: Dark mode aldagai faltsuak zuzendu `01-variables.css`, `02-material-overrides.css` eta `06-mermaid-diagrams.css`-en
- :material-bug: `registry` → `register` zuzenketaren masibo `registro-oficial.md`-n (es/en/eu)
- :material-web: Manrope tipografia tokiko fontetik Google Fonts-era migratuta

---

## v0.3.32 <small>— 2026-06-26</small> { #v0332 }

**Dokumentazio-hobekuntzak:**

- :material-palette: DENA kolore korporatiboen aplikazio osoa (#1D3328)
- :material-image: DENA logoa 64px-era tamainatu ikusgarritasun hobea lortzeko
- :material-view-grid: "DENA Operatiboak" sarrera-orria nabigazio bisualarekin
- :material-cogs: Atal operatiboen bateratzea menu nagusian
- :material-email: Laguntza-kontaktuaren integrazioa (admin-digital-data-dena@ejie.eus)
- :material-alert: Biltegi-bertsioaren oharrak dokumentazio osoan
- :material-star: Ikusizko koherentzia Material Design ikonoekin
- :material-navigation: Nabigazio sinplifikatua emojiak gabe
- :material-help: Kontaktu eta laguntza orri dedikatu bat

---

## v0.3.31 <small>— 2026-06-25</small> { #v0331 }

**Dokumentazio-hobekuntzak:**

- :material-refresh: `mkdocs-vars.yaml`-en eguneratutako aldagai zentralizatuak
- :material-refresh: Footer guztiak `{{ dena.version }}` eta `{{ dena.date }}` aldagaietara migratuta
- :material-refresh: Dokumentazio semantikoko bertsio-goiburuak aldagaiak erabiliz
- :material-plus: Mantentze-erraztasuna hobetua: bertsio-aldaketak fitxategi bakar batetik
- :material-bug: Dokumentazioko bertsioketa-inkoherentzien zuzenketa

---

## v0.3.26 <small>— 2026-06-11</small> { #v0326 }

**Dokumentazio-hobekuntzak:**

- :material-plus: Sarrera-orria berriro diseinatua Material for MkDocs-ekin
- :material-plus: "Hasteko gida" atala instalazio, komunikazio eta mock-arekin
- :material-plus: FAQ (Maiz egiten diren galderak)
- :material-plus: Terminoen glosarioa
- :material-plus: CSS pertsonalizatua DENA kolore korporatiboekin
- :material-plus: Laburdura teknikoetako aholkuak (OID, DIR3, SIA...)
- :material-plus: Aldagai berrerabilgarriak (`mkdocs-vars.yaml`)
- :material-refresh: Orri guztiak eguneratuta Material ezaugarriekin (admonitions, tabs, mermaid, grid cards)
- :material-refresh: Nabigazioa berrantolatuta fitxekin eta atal hedagarriekin

---

## v0.3.25 <small>— 2026-06-10</small> { #v0325 }

**Edukia:**

- :material-plus: DATA-RETRIEVE dokumentazio osoa (endpoint, objektuak, baliozkotzeak, erroreak, snippets)
- :material-plus: PERSON-SYNC dokumentazioa (Pull + Push, endpoints, ereduak)
- :material-plus: METADATA-SYNC dokumentazioa (endpoint)
- :material-plus: Data-Retrieve inplementazio-gida
- :material-plus: Mermaid diagrama interaktiboak semantikan
- :material-plus: Postman bildumak eguneratuta

---

## v0.2.0 <small>— 2026-03-15</small> { #v020 }

**Edukia:**

- :material-plus: Autentifikazio-dokumentazioa (Bezeroa ↔ DENA, Admin ↔ DENA)
- :material-plus: OAuth2 fluxu-diagramak
- :material-plus: get-token endpoint-a
- :material-plus: Oinarrizko semantika (REST Message egitura)

---

## v0.1.0 <small>— 2025-12-01</small> { #v010 }

**Edukia:**

- :material-plus: Biltegiko hasierako egitura
- :material-plus: Arkitektura orokorra (draw.io diagrama)
- :material-plus: Lehen datu-ereduak (DataTypeRef, PersonRef, OrgAdminRef)
- :material-plus: DevTools - DENA Admin Connection Test

---

## Bertsio-konbentzioa

| Formatua | Esanahia |
|---|---|
| `v0.X.Y` | Pre-release garapen-fasea |
| :material-plus: | Eduki berria |
| :material-refresh: | Eguneratutako edukia |
| :material-minus: | Ezabatutako edukia |
| :material-bug: | Errore-zuzenketa |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
