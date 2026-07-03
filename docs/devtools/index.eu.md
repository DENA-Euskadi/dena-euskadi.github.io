# :material-wrench: DevTools

Administrazio publikoentzat DENA-rekin probak eta integrazioa errazten dituzten garapen-tresnak.

---

## Tresna eskuragarriak

<div class="grid cards" markdown>

-   :material-web:{ .lg .middle } **DENA DevTools Services**

    ---

    Web tresna (Postman modukoa) DENA azpiegituratik HTTP endpoint-ak probatzeko.

    [:octicons-arrow-right-24: Ikusi behean](#dena-devtools-services)

-   :material-connection:{ .lg .middle } **DENA Admin Connection Test**

    ---

    Admin ↔ DENA norabide biko konektibitatea balidatzeko osagai hedagarria.

    [:octicons-arrow-right-24: Biltegia]({{ repos.conx_test_tree }})

</div>

---

## DENA DevTools Services

### Ingurune eskuragarriak

=== ":material-earth: Internet"

    | Ingurunea | URLa |
    |---|---|
    | **PRE** | [https://api-batera.pre.dena.eus/devtools-services/](https://api-batera.pre.dena.eus/devtools-services/) |
    | **PRO** | [https://api-batera.pro.dena.eus/devtools-services/](https://api-batera.pro.dena.eus/devtools-services/) *(laster)* |

=== ":material-lan: Euskalsarea"

    | Ingurunea | URLa |
    |---|---|
    | **PRE** | [https://api-batera.pre.batera.euskalsarea.eus/devtools-services/](https://api-batera.pre.batera.euskalsarea.eus/devtools-services/) |
    | **PRO** | [https://api-batera.pro.batera.euskalsarea.eus/devtools-services/](https://api-batera.pro.batera.euskalsarea.eus/devtools-services/) *(laster)* |

### Funtzionalitateak

- :material-swap-horizontal: **HTTP Metodoak** — GET, POST, PUT, DELETE, PATCH
- :material-shield-key: **Autentifikazioa** — Bearer Token, Basic Auth, API Key
- :material-code-json: **Body** — JSON, Form Data, URL Encoded, Raw Text
- :material-format-list-bulleted: **Goiburuak** — Goiburuen konfigurazio osoa
- :material-magnify: **Query parametroak** — Parametroak automatikoki gehitu
- :material-monitor-dashboard: **Erantzun xehatua** — Status code, goiburuak eta body
- :material-certificate: **SSL/TLS malgua** — Fidagaitzak diren ziurtagirientzako euskarria
- :material-text-box-search: **Logging** — Trazabilitate osoa UUID-arekin eskaera bakoitzeko

### REST APIa

!!! example "POST /api/devtools/test-endpoint"

    === "Eskaera"

        ```json
        {
          "method": "POST",
          "url": "https://api.administrazioa.com/endpoint",
          "headers": {
            "Content-Type": "application/json",
            "Authorization": "Bearer <token>"
          },
          "body": "{\"key\": \"value\"}"
        }
        ```

    === "Erantzuna"

        ```json
        {
          "statusCode": 200,
          "responseBody": "...",
          "responseHeaders": {...},
          "success": true
        }
        ```

### Erabilera kasuak

| Kasua | Deskribapena |
|---|---|
| DENA-tik probak | Tanzú-tik administrazioetara konektibitatea probatu |
| Endpoint-en baliozkotzea | Administrazioaren zerbitzuak eskuragarri daudela egiaztatu |
| Debugging | Konektibitate edo formatu arazoak diagnostikatu |
| Autentifikazio probak | Token-ak, ziurtagiriak eta kredentzialak balidatu |

---

## DENA Admin Connection Test

!!! info ""

    **Biltegia:** [DENA-Euskadi/dena-admin-conx-test]({{ repos.conx_test_tree }})

    Administrazioaren azpiegituran hedatzeko tresna, norabide biko konektibitatea balidatzeko.

    [:octicons-arrow-right-24: Erabilera gida osoa](../guia-inicio/probar-comunicaciones.md)

**Baliozkotzeak:**

- :material-check: DENA endpoint-ekiko konektibitatea
- :material-check: OAuth2 autentifikazio konfigurazioa
- :material-check: Eskaera/erantzunen formatua eta egitura
- :material-check: Ziurtagiriak eta sare konfigurazioa

---

## Eskakizunak

| Eskakizuna | Xehetasuna |
|---|---|
| :fontawesome-brands-java: Java | 21+ |
| :material-shield-key: OAuth2 kredentzialak | DENA-k emandakoak |
| :material-lock: HTTPS konektibitatea | DENA inguruneetara |

---

## Laguntza

| Kanala | Kontaktua |
|---|---|
| :material-book-open-variant: Dokumentazioa | [Dokumentazio hau](../index.md) |
| :material-bug: Arazoak | Dagokion biltegian jakinarazi |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
