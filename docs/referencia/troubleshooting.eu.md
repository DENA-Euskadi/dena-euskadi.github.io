# :material-lifebuoy: Troubleshooting

Ohiko errore eta haien konponbideen gida zentralizatua, kategoriaka antolatua.

---

## Konektibitatea

??? failure "Connection refused"

    **Sintoma:** Ezin da konexiorik ezarri endpoint-arekin.

    **Kausa posibleak:**

    - Zerbitzua ez dago martxan
    - Portua firewallak itxita
    - IP/hostname okerra

    **Konponbidea:**

    ```bash
    # Egiaztatu zerbitzua martxan dagoela
    curl -v http://localhost:8082/api/hello

    # Egiaztatu sare-konektibitatea
    telnet api-batera.pre.dena.eus 443

    # Egiaztatu DNS ebazpena
    nslookup api-batera.pre.dena.eus
    ```

??? failure "Connection timeout"

    **Sintoma:** Konexioa hasten da baina ez da espero den denboran osatzen.

    **Kausa posibleak:**

    - Firewalla isilik blokeatzen
    - Proxy korporatiboa konfiguratu gabe
    - Sare-arauak ireki gabe

    **Konponbidea:**

    - Egiaztatu firewall-arauak azpiegitura-taldearekin
    - Konfiguratu proxy-a: `export https_proxy=http://proxy:3128`
    - Eskatu trafiko-irekiera DENAren IP-etara

??? failure "SSL/TLS handshake error"

    **Sintoma:** `javax.net.ssl.SSLHandshakeException` edo antzekoa.

    **Kausa posibleak:**

    - Zerbitzariaren ziurtagiria ez da Java-ren truststore-ak ezagutzen
    - TLS bertsio bateraezina
    - Ziurtagiria iraungita

    **Konponbidea:**

    ```bash
    # Deskargatu ziurtagiria
    openssl s_client -connect api-batera.pre.dena.eus:443 < /dev/null 2>/dev/null | \
      openssl x509 > dena-ca.crt

    # Inportatu truststore-ra
    keytool -import -trustcacerts -alias dena-ca \
      -file dena-ca.crt \
      -keystore $JAVA_HOME/lib/security/cacerts \
      -storepass changeit
    ```

---

## Autentifikazioa

??? failure "401 Unauthorized"

    **Sintoma:** `{"error": "invalid_token"}` edo HTTP 401 erantzuna.

    **Kausa posibleak:**

    - Tokena iraungita
    - Ingurune okerreko tokena (PRE-ko tokena PRO-n erabilia)
    - Goiburuan gaizki osatutako tokena

    **Konponbidea:**

    - Egiaztatu tokena ez dela iraungitu (`expires_in`)
    - Birsortu tokena endpoint zuzenarekin
    - Egiaztatu goiburuaren formatua: `Authorization: Bearer <token>` (Bearer-en ondoren zuriunearekin)

??? failure "invalid_client tokena lortzean"

    **Sintoma:** `{"error": "invalid_client", "error_description": "Invalid client or Invalid client credentials"}`

    **Kausa posibleak:**

    - `client_id` okerra
    - `client_secret` okerra
    - Beste ingurune bateko kredentzialak

    **Konponbidea:**

    - Egiaztatu `client_id` eta `client_secret` (zuriunerik gabe)
    - Berretsi kredentzialak ingurune zuzenekoak direla (PRE vs PRO)
    - Jarri harremanetan DENA taldearekin kredentzialak funtzionatzen ez badute

??? failure "403 Forbidden"

    **Sintoma:** Token balioduna baina baimenik gabe.

    **Kausa posibleak:**

    - Bezeroak ez ditu beharrezko scope/rolak
    - Baliabideak baimen gehigarriak eskatzen ditu

    **Konponbidea:**

    - Jarri harremanetan DENA taldearekin bezeroaren baimenak berrikusteko

---

## Data-Retrieve

??? failure "400 Bad Request — derrigorrezko eremuak"

    **Sintoma:** `{"code": "CLIENT_ERR", "details": "Missing required fields..."}`

    **Kausa posibleak:**

    - `context.messageType` falta da
    - `context.subjectPerson.personId` falta da
    - `context.dataType.dataTypeId` falta da
    - `context.flowDirection` falta da

    **Konponbidea:**

    Egiaztatu JSON-ak derrigorrezko eremu guztiak dituela:

    ```json
    {
      "context": {
        "messageType": "PERSON_FETCH_DATA",
        "dataType": { "dataTypeId": "RECORDS" },
        "messageCorrelationId": "uuid-here",
        "flowDirection": "REQUEST",
        "subjectPerson": { "personId": "12345678A" }
      },
      "data": {}
    }
    ```

??? failure "Erantzun hutsa (dataItems: [])"

    **Sintoma:** HTTP 200 baina daturik gabe.

    **Kausa posibleak:**

    - Pertsonak ez ditu eskatutako motako datuak
    - `personId` ez dago administrazioaren sisteman
    - `dataTypeId` ez da ezagutzen

    **Konponbidea:**

    - Egiaztatu `personId` zure sisteman existitzen dela
    - Egiaztatu `dataTypeId` zure administrazioak kudeatzen dituen motekin bat datorrela
    - Portaera zuzena izan daiteke (espedienteik gabeko pertsona, adibidez)

??? failure "Erantzunaren timeout-a"

    **Sintoma:** DENAk timeout jasotzen du zure endpoint-a deitzean.

    **Kausa posibleak:**

    - Datu-baseko kontsulta motela
    - Zerbitzua gainkargatuta
    - Timeout-a oso baxu konfiguratuta

    **Konponbidea:**

    - Optimizatu datu-baseko kontsultak
    - Timeout estandarra 30s da. Beti erantzun tarte horren barruan
    - Denbora gehiago behar baduzu, jarri harremanetan DENA taldearekin

---

## Traffic Flow (DENA goiburuak)

??? failure "Missing required context fields"

    **Sintoma:** `{"status": 400, "message": "Missing required context fields: context.messageType..."}`

    **Kausa posibleak:**

    - Body-ak ez du `context` eremua
    - `context`-en barruan derrigorrezko eremuak falta dira

    **Konponbidea:**

    Body-ko derrigorrezko eremuak:

    - `context.messageCorrelationId`
    - `context.messageType`
    - `context.flowDirection`
    - `context.originPartyId`
    - `context.destinationPartyId`

??? failure "Hash mismatch (X-DENA-Data-Digest)"

    **Sintoma:** DENA goiburuetako hash okerragatik baztertua.

    **Kausa posibleak:**

    - Body-a aldatu da hasha kalkulatu ondoren
    - Proxy/middleware-ak body-a aldatzen du
    - Kodeketa okerra (UTF-8 espero da)

    **Konponbidea:**

    - Kalkulatu hasha sarean bidaltzen den body zehatzaren gainean
    - Ez aldatu body-a digestua sortu ondoren
    - Egiaztatu ez dagoela edukia aldatzen duen proxy bitartekorik

---

## Konpilazioa eta hedapena

??? failure "BUILD FAILURE — ebatzi gabeko mendekotasunak"

    **Sintoma:** Maven-ek ezin ditu DENA/R01F artefaktuak aurkitu.

    **Konponbidea:**

    Egiaztatu `settings.xml`:

    ```xml
    <repository>
      <id>ejie-group</id>
      <url>https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-group/</url>
    </repository>
    ```

??? failure "java.lang.UnsupportedClassVersionError"

    **Sintoma:** Errorea Java bertsio okerrarekin exekutatzean.

    **Konponbidea:**

    ```bash
    # Egiaztatu bertsioa
    java -version  # 21+ izan behar du

    # Egiaztatu JAVA_HOME
    echo $JAVA_HOME
    ```

---

!!! tip "Ez duzu zure errorea aurkitzen?"

    Arazoa irauten badu edo gida honetan zure errore zehatza aurkitzen ez baduzu:
    
    **:material-email: Jarri harremanetan DENA laguntza-taldearekin:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    Sartu zure kontsultan:
    
    - Errore-mezu osoa
    - Log garrantzitsuak
    - Ingurunea (PRE/PRO/local)
    - `messageCorrelationId` baduzu
    - Testuinguru-informazioa (zer egin nahi zenuen?)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
