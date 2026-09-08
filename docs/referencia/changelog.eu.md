# :material-history: Changelog

DENA dokumentazioaren bertsio-historia.

---

## v0.5.2 <small>— 2026-09-07</small> { #v052 }

!!! success "Unekoa"

**Dokumentazioa DENA API komunaren 0.4.16 kode-berrikuspenarekin sinkronizatuta.**

Eduki berria:

- :material-plus: `arquitectura/arquitectura-servicios.md`: bezeroaren REST proxy oinarria (`DN00ClientAPIRESTServiceProxyBase`) eta CORE proxyen marka-interfazea (`DN00IsDENACOREServiceProxy`) dokumentatuta, haien portaerarekin (JSON marshalling, birsaiakera konfiguragarriak)
- :material-plus: Security context-aren erabiltzaile-ereduaren orri berria (`seguridad/modelo-usuarios.md`): administrazioaren sistema-erabiltzaile berria (`DN00DENAAdminSystemUser`) dokumentatuta, pertsona- eta kudeaketa-erabiltzaileekin batera

Zuzenketak 0.4.16 kodearen aurrean:

- :material-refresh: Refs eredua (object-ref, person-ref, org-admin-ref, data-type-ref): benetako eremuak `oid`/`id`/`dir3Id`; existitzen ez ziren eremuak kenduta (`objectOid`, `createTS`, `lastUpdateTS`, `deleteTS`, `url`, `personId`, `orgId`, `officialId`)
- :material-refresh: Interop message eredua: benetako egitura `context` + `protocol` + `payload`; `context.message.{type,correlationId,interopRouteData}`; `flowDirection` eratorria; sinplifikazioa (`__DN00IsInteropMessagePayload` kenduta)
- :material-refresh: Data-retrieve datu-eredua: `originAdmin`/`aboutPerson` (lehen `*Ref`), `lastChangedAt` gehituta
- :material-refresh: Oinarrizko semantika: `status` (code/errorId/details), `consentOid`, `sync-types` (`DN00SyncMetaDataFromAdminToCOREItem`), http-headers eta language-texts kodearekin lerrokatuta
- :material-refresh: Kode-estekak eta JSON adibideak (ES/EN/EU) benetako eredura eguneratuta

Itzulpenak:

- :material-translate: EN eta EU-ra itzuli dira ES-en soilik zeuden orriak (arquitectura-servicios, configuracion, tipos-dato-base, data-retrieve/index)

---

## v0.5.1 <small>— 2026-09-08</small> { #v051 }

**Person-Sync dokumentazio osoa DENA-Architecture.docx eta DENA-CORE-Services_for_admins.docx-etik ateratakoa.**

Arkitektura-eduki berria:

- :material-plus: "Person-Sync" atala hedatua Push, Pull On-line eta Pull Off-line-rekin
- :material-plus: Mermaid fluxu-diagramak Bespoke job flow-rako
- :material-plus: JSON payload adibideak (Push, Pull On-line, Bespoke requests)
- :material-plus: Job-aren egoerak dokumentatuta: REGISTERED, BEING_PROCESSED, FINISHED_OK, FINISHED_NOK
- :material-plus: Java API adibideak arquitectura-servicios.md-rako
- :material-plus: Word-etik ateratako irudiak: image18.png (Person-Sync Overview), image7.png (DENA Push)

Itzulpenak:

- :material-translate: arquitectura/index.md: eduki osoa EN eta EU-ra itzulita
- :material-translate: arquitectura/arquitectura-servicios.md: Java adibideak itzulita

Garbiketa:

- :material-minus: image8.png kenduta Admin Pull-etik (ez zegokion testuinguruari)
- :material-minus: arquitectura-dena-completa.md aldi baterako fitxategia arquitectura/index.md-n bateratua

---

## v0.5.0 <small>— 2026-08-24</small> { #v050 }

**Dokumentazioaren hobekuntza integrala: egitura, edukia, koherentzia eta itzulpenak.**

Zuzendutako inkoherentziak:

- :material-bug: "Informazio gehiago" atala hirukoiztuta arquitectura/index.md-n
- :material-bug: `REGISTRY` zuzenduta `REGISTER`-era (enum-aren balio kanonikoa) 6 fitxategitan
- :material-bug: Kolore-legenda okerra semantica/index-en (morea → horia)
- :material-bug: Data zaharkitua sistema-versionado.md-n
- :material-bug: TRANSLATION_TRACKER kontagailu kontraesankorrekin

Eduki berria:

- :material-plus: End-to-End adibide osoa (token + SRMD + Data-Retrieve + bash script)
- :material-plus: APIaren muga eta murrizketen orria (timeout dokumentatuak)
- :material-plus: Java Spring Boot kode-adibideak (controller, entity, metadata-sync service, token service)
- :material-plus: Glosarioa hedatua 6 terminorekin: Cold-Start, Konektore, Data Origin Instance, DENA-APP, DENA-CORE, SRMD

Segurtasun eta autentifikazioaren berregituraketa:

- :material-refresh: "Zure Sistemak DENA Deitzen Du" get-token + segurtasun-goiburuak integratzen ditu (eremuen sorrera)
- :material-refresh: "DENAk Zure Sistema Deitzen Du" JWT eredua + zerbitzuak + mekanismo alternatiboak integratzen ditu (OAuth, mTLS, CAS, API Key, Basic Auth, WS-Security)
- :material-minus: modelo.md, servicios.md eta endpoint-get-token.md kenduta (orri nagusietan integratuta)

Lehendik zegoen edukiaren hobekuntzak:

- :material-refresh: Data-Retrieve: adibide-JSONa interop egitura osoarekin (tolesgarria)
- :material-refresh: Data-Retrieve: "Erreferentzia-kodea" atala test-etarako eta DN00InteropHeaders-erako estekekin
- :material-refresh: Metadata-Sync: "zer aldatu zen eta noiz" zehaztapena (ez nork)
- :material-refresh: Metadata-Sync: fromDataOrigin, IDs vs OIDs, data origin errore klasikoaren dokumentazioa
- :material-refresh: ES/EN/EU index bateratua egitura narratibo berdinarekin

Hizkuntza inklusiboa:

- :material-refresh: Iragazketa osoa: "erabiltzaile" → "pertsona", "herritar" → "pertsona herritarra/herritartasuna"
- :material-refresh: ES, EN eta EU-n aplikatua (citizen → person, herritarra → pertsona)

Garbiketa:

- :material-minus: ADR hutsak kenduta (0001, 0002)
- :material-minus: Argitaratu gabeko legacy fitxategiak kenduta (vision-general-detallada, seguridad, servicios-core-admins)
- :material-minus: Itzulpen zaharkituak kenduta eta birsortuta

Itzulpenak:

- :material-translate: EN+EU itzulpen berriak edo eguneratuak aldatutako orri guztietarako
- :material-translate: operativas/data-retrieve, metadata-sync, person-sync (EN+EU)
- :material-translate: autenticacion/administracion-core-dena, core-dena-administracion (EN+EU)
- :material-translate: ejemplo-end-to-end, limites-api (EN+EU)

---

## v0.4.0 <small>— 2026-08-24</small> { #v040 }

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

- :material-palette: CSS fitxategi zenbakituetan (`01`–`06`) berrantolatuta erantzukizunaren arabera
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
