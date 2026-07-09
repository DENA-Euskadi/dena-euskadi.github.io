# :material-frequently-asked-questions: Maiz egiten diren galderak (FAQ)

Administrazio integratzaileen zalantza ohikoenei erantzunak.

---

## Integrazio orokorra

??? question "Zer behar dut DENArekin integratzen hasteko?"

    1. **OAuth2 kredentzialak** DENA taldeak emandakoak (`client_id` + `client_secret`)
    2. **HTTPS konektibitatea** DENA endpoint-etara (PRE eta/edo PRO)
    3. **Java 21+** eta **Maven 3.9+** adibide/test proiektuak konpilatzeko

    [:octicons-arrow-right-24: Instalazio-gida](../guia-inicio/instalacion.md)

??? question "Zein endpoint inplementatu behar ditut derrigorrez?"

    Zure erabilera-kasuaren araberakoa da:

    | Kasua | Derrigorrezko endpoint-a |
    |---|---|
    | DENAk zure administrazioko datuak kontsultatzen ditu | `POST /api/retrieveData` ([Data-Retrieve](../semantica/data-retrieve/index.md)) |
    | DENAri aldaketak jakinarazten dizkiozu | `POST` DENAren [Metadata-Sync](../semantica/metadata-sync/index.md) endpoint-era |
    | Pertsonak Push bidez jasotzen dituzu | `POST /api/person-push` ([Person-Sync Push](../semantica/person-sync/push.md)) |
    | Pertsonak Pull bidez deskargatzen dituzu | Zurerik ez, DENAri deitu besterik ez |

??? question "Nire sistema Java ez bada integra al naiteke?"

    Bai. DENAk **REST + JSON estandarra** erabiltzen du. HTTP POST eskaerak egin eta JSON itzul dezakeen edozein hizkuntza bateragarria da.

    [Kode-zatiak](../semantica/data-retrieve/snippets-codigo.md) daude Java, C#, Python, Node.js eta PHP-n.

??? question "Zer gertatzen da endpoint estandarra inplementatu ezin badut?"

    DENAk zure sistemara egokitutako **konektore pertsonalizatu** bat garatuko du (SOAP, fitxategiak, API propioa...).
    Koordinatzeko, jarri harremanetan DENA taldearekin.

---

## Autentifikazioa

??? question "Nola lortzen ditut nire OAuth2 kredentzialak?"

    Kredentzialak (`client_id` eta `client_secret`) DENA taldeak ematen ditu onboarding prozesuan.
    Administrazio eta ingurune bakoitzarentzat espezifikoak dira (PRE/PRO).

??? question "Zenbat irauten du tokenak? Berritu egin behar al dut?"

    Normalean **5 minutu** (`expires_in: 300`). Gomendatzen da:

    - Tokena cachean gordetzea baliozkoa den bitartean
    - Iraungitzeko ~60 segundo lehenago berritzea (leeway)
    - Ez eskatzea token berri bat eskaera bakoitzean

    [:octicons-arrow-right-24: get-token endpoint-a](../autenticacion/administracion-core-dena/endpoint-get-token.md)

??? question "Nire IDP propioa erabil al dezaket (Keycloak, ADFS, Cognito)?"

    Bai. DENA zure administrazioari deitzen dionean (Data-Retrieve), zuk ematen dituzu kredentzialak eta zure IDP-ren URLa.
    DENAk `client_credentials` erabiliko du zu deitu aurretik tokena lortzeko.

    [:octicons-arrow-right-24: CORE DENA → Administrazioa](../autenticacion/core-dena-administracion/index.md)

---

## Data-Retrieve

??? question "Zer itzultzen dut pertsona horren daturik ez badut?"

    HTTP **200** `dataItems` hutsarekin:

    ```json
    {
      "context": { ... },
      "data": { "dataItems": [] },
      "code": "OK"
    }
    ```

    :material-alert: **Inoiz ez** itzuli 404 "daturik ez" adierazteko. 404 soilik da "pertsona ez dago nire sisteman".

??? question "Datu partzialak itzul al ditzaket mota batzuk bakarrik baditut?"

    Bai. Itzuli eskatutako `dataTypeId`-rako eskuragarri dituzun objektuak bakarrik.
    `RECORDS` eskatzen badizute eta pertsona horren espedienteik ez baduzu, itzuli `dataItems: []`.

??? question "Hizkuntz anitzeko testuak derrigorrez euskaraz eta gaztelaniaz egon behar al dira?"

    **Oso gomendagarria** da bi hizkuntzak sartzea (`SPANISH` + `BASQUE`).
    Hizkuntza bat bakarrik baduzu, sartu gutxienez hori. Bezero-aplikazioak eskuragarri dagoena erakutsiko du.

??? question "Zenbat denbora daukat erantzuteko?"

    Timeout estandarra **30 segundokoa** da. Zure sistemak kontsulta konplexuetarako denbora gehiago behar badu,
    jarri harremanetan DENA taldearekin timeout luzatua konfiguratzeko.

---

## Person-Sync

??? question "Pull ala Push? Zein aukeratzen dut?"

    | Irizpidea | Pull | Push |
    |---|---|---|
    | Denbora errealeko datuak behar dituzu | :material-close: | :material-check: |
    | Zure sistemak batch-ean prozesatzen du | :material-check: | :material-close: |
    | Ezin dituzu endpoint-ak esposatu | :material-check: | :material-close: |
    | Berehala erreakzionatu nahi duzu | :material-close: | :material-check: |

    Biak aldi berean erabil ditzakezu.

??? question "Zein maiztasunekin sortzen dira Pull fitxategiak?"

    **Ordu** bakoitzean fitxategi inkremental bat sortzen da azken ordutik aldatutakoekin.
    Iragazki pertsonalizatuekin esportazio pertsonalizatuak ere eska ditzakezu.

---

## Konektibitatea eta sarea

??? question "Zein IP-etatik deitzen du DENAk nire sistemara?"

    Ingurunearen araberakoa da. Jarri harremanetan DENA taldearekin zure firewallean baimendu behar duzun IP-tartea lortzeko.

??? question "SSL ziurtagiria behar al dut nire endpoint-erako?"

    Bai. DENAk **HTTPS** bidez bakarrik konektatzen du. Zure endpoint-ak ziurtagiri baliodun bat izan behar du
    (CA ezagun batek edo euskalsarea-ren barne CA-k emandakoa, sare horretan bazaude).

??? question "Nola egiaztatzen dut konektibitatea martxan jarri aurretik?"

    Erabili [DENA Admin Connection Test]({{ repos.conx_test_tree }}) osagaia.

    [:octicons-arrow-right-24: Komunikazio-gida](../guia-inicio/probar-comunicaciones.md)

---

## Tresnak

??? question "Atzean benetako sistema bat izan gabe probatu dezaket?"

    Bai, erabili [Espedienteen Moka](../guia-inicio/mock-expedientes.md), adibide-datuekin administrazio bat simulatzen duena.

??? question "Postman bildumak al dituzue?"

    Bai. Eskuragarri [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman)-en.

    Data-Retrieve, Metadata-Sync, Person-Sync eta autentifikaziorako eskaerak dituzte.

??? question "Ba al dago DENA endpoint-entzako Swagger/OpenAPI bat?"

    Argitaratze-prozesuan dago. Bitartean, zehaztapen osoa dokumentazio honetan dago.

---

## Laguntza

??? question "Norekin jarri naiteke harremanetan integrazio arazo bat badut?"

    **:material-email: DENA laguntza-emaila:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    | Kontsulta mota | Zer sartu |
    |---|---|
    | **Galdera teknikoak** | Arazoaren deskribapena, log garrantzitsuak, ingurunea (PRE/PRO) |
    | **Konektibitate-arazoak** | Egindako testak, sare-konfigurazioa, errore-mezua |
    | **Kredentzialak/Sarbideak** | Eskatzen duen administrazioa, ingurunea, client_id baduzu |
    | **Akatsak/Arazoak** | Erreproduzitzeko urratsak, espero den portaera vs benetakoa |
    
    :material-clock: **Erantzun-denbora:** Lan-orduetan kontsulta larriak 4 ordu baino gutxiagoan artatu.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
