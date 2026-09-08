# :material-speedometer: APIaren mugak eta murrizketak

DENA APIaren ezagutzen diren parametro operatiboak.

---

## Timeout-ak

| Eragiketa | Timeout-a | Erreferentzia |
|-----------|-----------|---------------|
| **Data-Retrieve** (DENA → zure administrazioa) | 30 segundo | `protocol.timeOut: "30s"` eremua eskaeran |

!!! tip "Data-Retrieve: 30 segundo"
    Zure endpoint-ak 30 segundo baino gutxiagoan erantzun behar du. Kontsulta konplexuengatik denbora gehiago behar baduzu, jarri harremanetan DENA taldearekin zure konektorerako timeout luzatua konfiguratzeko.

---

## OAuth2 tokena

| Parametroa | Balioa | Erreferentzia |
|------------|--------|---------------|
| **Tokenaren iraupena** | 300 segundo (5 min) | `expires_in` eremua get-token endpoint-aren erantzunean |
| **Gomendatutako tartea** | ~60 segundo iraungitu aurretik | Zure Sistema DENAri Deitzen Dio atalean eta FAQ-ean dokumentatutako gomendioa |
| **Grant type** | `client_credentials` | Onartutako grant type bakarra |

---

## Dokumentatzeke dauden balioak

!!! warning "Definitzeke"
    Hurrengo parametro operatiboak ez daude gaur egun dokumentatuta. Jarri harremanetan DENA taldearekin informazio hau zure integraziorako behar baduzu:
    
    - **Rate limit-ak**: administrazio bakoitzeko minutuko/orduko eskaera kopuru maximoa
    - **Payload-aren gehieneko tamaina**: request eta response-etako body-aren tamaina muga
    - **Berreskaera politika**: DENAren portaera zure endpoint-ak erroreak itzultzen dituenean
    - **Erabilgarritasun SLA**: ingurunearen arabera (PRE/PRO) bermatutako uptime ehunekoa
    - **APIaren bersionatzea**: bateragarritasun politika eta breaking change-en jakinarazpena
    - **Mantentze leihoak**: ez-erabilgarritasun planifikatuko ordutegia

---

## Gomendio orokorrak

Jardunbide egokietan eta sistemaren dokumentatutako portaeran oinarrituta:

- **Cacheatu tokena** baliozkoa den bitartean. Ez eskatu token berria dei bakoitzean.
- **Berritu tokena** aldez aurretik (~60s iraungitu aurretik) sarearen latentziagatik errefusak saihesteko.
- **Erantzun azkar**: 30s-ko timeout-a maximoa da, baina zenbat eta azkarrago erantzun, orduan eta esperientzia hobea herritarrarentzat.
- **Itzuli beti HTTP 200** `dataItems: []` datuekin daturik ez dagoenean. Ez erabili 404.

---

!!! question "Mugei buruzko informazioa behar duzu?"
    
    Rate limit-ei, payload-aren gehieneko tamainari edo SLAri buruzko kontsultetarako:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
