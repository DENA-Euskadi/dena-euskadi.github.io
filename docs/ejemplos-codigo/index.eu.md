# :material-file-code: Kode Adibideak

Java erreferentzia proiektua administrazioentzako integrazio adibideekin.

---

!!! warning "Garapenean"

    Adibide proiektua prestatzen ari da. Hurrengo bertsioetan argitaratuko da.

---

## Aurreikusitako edukia

| Adibidea | Semantika | Deskribapena |
|---|---|---|
| Data-Retrieve oinarrizkoa | DATA-RETRIEVE | Espedienteak itzultzen dituen gutxieneko endpoint-a |
| Metadata-Sync | METADATA-SYNC | Aldaketen jakinarazpena DENA-ri |
| Person-Sync Pull | PERSON-SYNC | Pertsonen fitxategiaren deskarga |
| Person-Sync Push | PERSON-SYNC | Pertsonen jakinarazpenen jasoketa |
| OAuth2 Autentifikazioa | Autentifikazioa | Token-en lorpena eta erabilera |

---

## Adibideak exekutatzeko eskakizunak

| Tresna | Bertsioa |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-apachemaven: Maven | 3.9+ |
| :material-connection: Konektibitatea | DENA PRE-rantz |

---

## Nola erabili (eskuragarri dagoenean)

!!! warning "DENA biltegietan bertsio kontrola"
    
    DENA-ren edozein biltegi klonatzean, ziurtatu bertsio etiketa egokia erabiltzen duzula `git checkout <tag>` bidez, zure garapen eta hedapen ingurunearekin bateragarritasuna bermatzeko.

```bash
git clone <adibide-biltegiaren-url>
cd dena-data-codesamples
mvn clean package
java -jar target/<artefaktua>.jar
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
