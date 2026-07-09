---
hide:
  - toc
---

# DENA Interop — Administrazioentzako Dokumentazioa

<div class="grid cards" markdown>

-   :material-rocket-launch:{ .lg .middle } **Lehen aldia hemen?**

    ---

    Egiaztapen-zerrenda osoa: ingurunea instalatzetik zure lehen integrazioa martxan jarri arte.

    [:octicons-arrow-right-24: onboardinga](./guia-inicio/onboarding.md)

-   :material-swap-horizontal:{ .lg .middle } **Integrazioa inplementatu**

    ---

    Zure administrazioak DENArekin komunikatzeko erakutsi behar duen endpoint estandarra.

    [:octicons-arrow-right-24: Semantika](./semantica/index.md)

-   :material-shield-lock:{ .lg .middle } **Autentifikazioa konfiguratu**

    ---

    OAuth2 fluxuak zure sistemaren eta DENAren artean (client_credentials).

    [:octicons-arrow-right-24: Autentifikazioa](./autenticacion/index.md)

-   :material-wrench:{ .lg .middle } **Proba-tresnak**

    ---

    DevTools, Postman bildumak eta norabide biko konektibitate-probak.

    [:octicons-arrow-right-24: DevTools](./devtools/index.md)

</div>

---

## Zer da DENA?

**DENA** Eusko Jaurlaritzaren elkarreragingarritasun-plataforma da, herritarrei aplikazio bakar batetik sarbidea ematen diena administrazio publikoek haiei buruz kudeatzen dituzten datuetara.

``` mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#1a4d1f"
    primaryBorderColor: "#70d680"
    lineColor: "#1a4d1f"
    fontSize: "14px"
    fontFamily: "Manrope, sans-serif"
---
graph LR
    A[Erabiltzailea] -->|DENA App| B(DENA CORE)
    B -->|Data-Retrieve| C[A Administrazioa]
    B -->|Data-Retrieve| D[B Administrazioa]
    C -->|Metadata-Sync| B
    D -->|Metadata-Sync| B
    B <-->|Person-Sync| C
    B <-->|Person-Sync| D
    
    style A fill:#f5d836,stroke:#1a4d1f,color:#1a4d1f,stroke-width:2px
    style B fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style C fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
    style D fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## :material-map-marker-path: Zer egin behar duzu?

=== "Datuak DENAri eman"

    Zure administrazioak REST endpoint bat erakusten du DENAk pertsona baten datuak kontsulta ditzan.

    **Endpoint-a:** `POST /api/retrieveData`

    [:octicons-arrow-right-24: Data-Retrieve Dokumentazioa](./semantica/data-retrieve/index.md)

=== "Aldaketak jakinarazi"

    Zure sistemak DENAri jakinarazten dio pertsona batentzat datu berriak daudela eskuragarri.

    **Endpoint-a:** `POST /api/syncMetadata`

    [:octicons-arrow-right-24: Metadata-Sync Dokumentazioa](./semantica/metadata-sync/index.md)

=== "Pertsonak sinkronizatu"

    DENAren eta zure administrazioaren artean erregistratutako pertsonen zerrenda eguneratuta mantendu.

    **Mekanismoak:** Pull (zuk kontsultatzen duzu) / Push (DENAk jakinarazten dizu)

    [:octicons-arrow-right-24: Person-Sync Dokumentazioa](./semantica/person-sync/index.md)

=== "Konektibitatea probatu"

    Zure azpiegituraren eta DENAren arteko norabide biko komunikazioa funtzionatzen duela balioztatu.

    ```bash
    curl -X POST http://localhost:8082/api/conxTest \
      -H "Content-Type: application/json" \
      -d '{"environment": "PRE"}'
    ```

    [:octicons-arrow-right-24: Komunikazioak gida](./guia-inicio/probar-comunicaciones.md)

---

## :material-lightning-bolt: Hasiera azkarra

!!! warning "Biltegiaren bertsioa egiaztatu"
    
    Ziurtatu biltegiaren bertsio zuzena erabiltzen duzula klonazioarekin aurrera egin aurretik. Lan-ingurune honetarako gomendatutako bertsioa biltegian egonkor gisa etiketatutakoa da.

!!! tip "5 minutu zure ingurunea baliozkotu"

    ```bash
    # 1. Konektibitate testa klonatu
    git clone {{ repos.conx_test_clone }}
    cd dena-admin-conx-test

    # 2. Konpilatu eta abiarazi
    mvn clean package -Pstandalone
    java -jar denaAdminConxTestRESTApp/target/denaAdminConxTestRESTApp-*.war

    # 3. Egiaztatu
    curl http://localhost:8082/api/hello

    # 4. DENA PREren aurkako proba
    curl -X POST http://localhost:8082/api/conxTest \
      -H "Content-Type: application/json" \
      -d '{"environment": "PRE"}'
    ```

---

## :material-sitemap: Dokumentazio mapa

| Atala | Edukia |
|---|---|
| [:material-play-circle: Hasierako gida](./guia-inicio/onboarding.md) | onboardinga, instalazioa, komunikazioak, mock |
| [:material-cube-outline: Arkitektura](./arquitectura/index.md) | Ikuspegi orokorra, diagramak, sistemaren moduluak |
| [:material-shield-lock: Autentifikazioa](./autenticacion/index.md) | OAuth2 client_credentials, Admin ↔ DENA fluxuak |
| [:material-code-braces: Semantika](./semantica/index.md) | Data-Retrieve, Metadata-Sync, Person-Sync |
| [:material-wrench: DevTools](./devtools/index.md) | DENAtik HTTP probak egiteko web tresna |
| [:material-book-open-variant: Erreferentzia](./referencia/faq.md) | FAQ, Glosarioa, Troubleshooting, Changelog, Matrizea |
| [:material-file-code: Adibideak](./ejemplos-codigo/index.md) | Java erreferentzia proiektua |
| [:material-paperclip: Eranskinak](./adjuntos/index.md) | Postman bildumak, inguruneak, irudiak |

---

## :material-server-network: Teknologia pila

| Osagaia | Bertsioa |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :simple-spring: Spring Boot | 3.3.5 |
| :simple-apachemaven: Maven | 3.9+ |
| :material-code-json: Jackson | 2.19.x |

---

!!! info "DENA Inguruneak"

    | Ingurunea | Internet | Euskalsarea |
    |---|---|---|
    | **PRE** | `https://api-batera.pre.dena.eus` | `https://api-batera.pre.batera.euskalsarea.eus` |
    | **PRO** | `https://api-batera.pro.dena.eus` | `https://api-batera.pro.batera.euskalsarea.eus` |

!!! question "Laguntza teknikoa"
    
    Kontsulta teknikoetarako, integrazio arazoetarako edo kredentzial eskaeretarako:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

---

<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
