# :material-book-alphabet: Glosarioa

DENA dokumentazioan erabiltzen diren termino tekniko eta funtzionalen definizioa.

---

## A

`API`
:   Application Programming Interface. Aplikazioak programatzeko interfazea.

`ADR`
:   Architecture Decision Record. Arkitektura-erabaki baten erregistro formala.

---

## C

`client_credentials`
:   OAuth2 fluxua non zerbitzu batek bere `client_id` eta `client_secret`-ekin autentifikatzen den token bat lortzeko. Ez du erabiltzailearen esku-hartzerik behar.

`Cold-Start`
:   Pertsona bat DENAn lehen aldiz inskribatzen denean eta SRMDrik existitzen ez denean gertatzen den arazoa, administrazioek oraindik ez dutelako berari buruz jakin. DENA-COREk hasierako SRMDak txertatzen ditu admin/datu-mota nagusietarako, aplikazioak edukia erakuts dezan lehenengo unetik.

`Conector`
:   DENA arkitekturan DENA-CORE eta administrazio baten artean bitartekari gisa aritzen den modulu independentea. Barne-alde bat du (DENA semantika estandarra) eta kanpo-alde bat (adminaren semantika). Administrazioak formatu estandarra erabiltzen badu, konektorea gardena da; bestela, bi formatuen artean itzultzen du.

`consentOid`
:   Pertsona batek bere datuetarako sarbidea emateko emandako baimenaren identifikatzaile bakarra.

`correlationId`
:   Jatorrizko eskaera beretik eratorritako dei guztiak korrelazionatzeko aukera ematen duen UUID-a.

---

## D

`Data Origin Instance`
:   Administrazio batek datu-mota bererako datu-jatorri anitz dituenean erabiltzen den bideratze-identifikatzaile gehigarria (adib.: espediente kudeatzaile anitz). DENA-COREri eskaera konektore zuzenera bideratzeko aukera ematen dio.

`Data-Retrieve`
:   DENAk administrazio bati pertsona baten datuak eskatzen dizkion mekanismoa.

`dataItems`
:   Administrazioak Data-Retrieve erantzunean itzultzen dituen domeinu-objektuen arraya.

`DataTypeRef`
:   DENA datu-mota baterako erreferentzia-objektua (espedientea, jakinarazpena, ordainketa...).

`DENA`
:   Eusko Jaurlaritzaren elkarreragingarritasun-plataforma, pertsonei administrazioen datuetarako sarbidea errazten diena.

`DENA-APP`
:   Pertsonek beren datuetara sartzeko erabiltzen duten mugikor eta web aplikazioa. DENA-CORErekin komunikatzen da SRMD sinkronizatzeko eta administrazioetatik datuak berreskuratzeko.

`DENA-CORE`
:   DENA plataformaren sistema zentrala, aplikazioaren (DENA-APP) eta administrazioen artean bitartekari gisa aritzen dena. Autentifikazioa, konektoreak, SRMD biltegiratze eta Data-Retrieve eskaeren orkestrazioa kudeatzen ditu.

`DIR3`
:   Unitate Organiko eta Bulegoen Direktorio Komuna. Estatuko Administrazio Orokorraren katalogo ofiziala, unitate administratibo bakoitza modu bakarrean identifikatzen duena.

---

## E

`EJIE`
:   Eusko Jaurlaritzaren Informatika Elkartea. IKT azpiegitura kudeatzen duen erakundea.

`Euskalsarea`
:   Eusko Jaurlaritzaren sare korporatiboa. Internetaren alternatiba barne-komunikazioetarako.

---

## F

`flowDirection`
:   Interop mezu bat eskaera (`REQUEST`) ala erantzuna (`RESPONSE`) den adierazten duen eremua.

---

## G

`Giltza`
:   Eusko Jaurlaritzaren autentifikazio-sistema, ziurtagiri digitaletan eta cl@ve-n oinarritua.

---

## I

`IDP`
:   Identity Provider. Tokenak igortzen dituen identitate-hornitzailea (Keycloak, ADFS, Cognito...).

`interopRouteData`
:   Trazabilitate-arraya, mezu bat zein DENA osagaietatik igaro den eta noiz erregistratzen duena.

---

## J

`JWT`
:   JSON Web Token. Zerbitzuen arteko autentifikaziorako erabiltzen den digitalki sinatutako tokena.

---

## L

`LanguageTexts`
:   Hizkuntza anitzeko testuentzako gako-balio mapa-objektua. Gakoak: `SPANISH`, `BASQUE`, `ENGLISH`.

`leeway`
:   Token baten benetako iraungitzearen aurretik aplikatzen den segurtasun-tartea (segundotan), sare-latentziagatiko bazterketak saihesteko.

---

## M

`messageType`
:   Interop mezu baten `data` eremuan bidalitako mezu-mota (adib.: `PERSON_FETCH_DATA`, `CLIENT_LOGIN`).

`Metadata-Sync`
:   Administrazioek DENAri pertsonen datuetako aldaketak jakinarazten dizkioten mekanismoa.

---

## N

`NIF`
:   Identifikazio Fiskaleko Zenbakia. Espainiako pertsona fisiko eta juridikoak identifikatzen ditu.

`NIE`
:   Atzerritarren Nortasun Zenbakia. Espainiako atzerritar egoiliarrak identifikatzen ditu.

---

## O

`OAuth2`
:   Open Authorization 2.0. DENA zerbitzuen arteko komunikaziorako erabiltzen den baimen-protokolo estandarra.

`OID`
:   Object Identifier. Sistemak automatikoki esleitutako identifikatzaile tekniko bakarra. UUID formatua.

`OrgAdminRef`
:   Administrazio baterako erreferentzia-objektua bere `oid` edo `id` bidez.

---

## P

`Person-Sync`
:   DENAn erregistratutako pertsonen zerrendaren sinkronizazio-mekanismoa administrazioekin.

`PersonRef`
:   Pertsona baterako erreferentzia-objektua bere `oid` edo `id` (NIF/NIE) bidez.

`Pull`
:   Sinkronizazio-eredua non administrazioa DENAra konektatzen den pertsonen datuak deskargatzeko.

`Push`
:   Sinkronizazio-eredua non DENAk modu proaktiboan jakinarazten dion administrazioari pertsonetan dauden aldaketei buruz.

---

## R

`R01F`
:   Eusko Jaurlaritzaren oinarrizko garapen-framework-a (Fabric). Utilitateak, OID-ak, serializazioa eta zerbitzu komunak eskaintzen ditu.

`REST`
:   Representational State Transfer. HTTP-n oinarritutako web APIentzako arkitektura-estiloa.

---

## S

`SIA`
:   Administrazio Informazio Sistema. Estatuko Administrazio Orokorraren prozedura eta zerbitzuen katalogo ofiziala.

`SRMD`
:   *Sync and Retrieve Meta-Data*. Administrazio batek aldizka DENA-COREri bidaltzen dizkion abisuak, zenbait pertsonentzat datu berriak edo eguneratuak daudela adieraziz. Beren edukia: pertsona + datu-mota + admin + azken eguneratzearen unea. Ez dituzte datuak berak edukitzen, aldaketa bat egon dela jakinaraztea baizik.

`subjectPerson`
:   Datuak eskatzen edo bidaltzen diren pertsonari buruz identifikatzen duen interop testuinguru-eremua.

---

## T

`Tanzú`
:   DENA azpiegiturak erabiltzen duen edukiontzien zabalpen-plataforma (Kubernetes).

---

## U

`UUID`
:   Universally Unique Identifier. 128 biteko identifikatzailea `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` formatuan.

---

## W

`WebAuthn`
:   Web Authentication API. Kriptografia asimetrikoan oinarritutako pasahitzik gabeko autentifikaziorako W3C estandarra.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
