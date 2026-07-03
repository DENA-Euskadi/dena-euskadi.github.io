# :material-sync: METADATA-SYNC

> **Bertsioa:** `v{{ dena.version }}` · **Data:** {{ dena.date }}

---

## Zer da?

**Metadata-Sync** administrazioek DENA-ri jakinarazteko mekanismoa da, erabiltzaile bati lotutako edozein datutan aldaketak gertatzen direnean.

``` mermaid
sequenceDiagram
    participant Admin as Administrazioa
    participant DENA as CORE DENA
    participant App as Bezero-aplikazioa

    Admin->>DENA: POST /syncMetadata (X pertsonak aldaketak ditu)
    DENA-->>Admin: 200 OK

    Note over DENA: Metadatua gordetzen du: pertsona + mota + data

    App->>DENA: Berrikuntzarik al dago?
    DENA-->>App: Bai, Admin X-ek datu berriak ditu zuretzat
```

!!! info "Metadatuak soilik"

    DENA-k **ez ditu datuak berak gordetzen**, azken eguneratzearen data bakarrik, konbinazio honen arabera:

    - Pertsona
    - Datu mota
    - Administrazioa

    Bezero-aplikazioak benetako datuak behar dituenean, [Data-Retrieve](../data-retrieve/index.md) bidez eskatuko ditu.

---

## Dokumentazioa

| Dokumentua | Edukia |
|---|---|
| [:octicons-arrow-right-24: Endpoint-a](./endpoint-sync-metadata.md) | Aldaketa-jakinarazpenerako REST kontratua |

---

!!! tip "Postman"

    Postman bilduma eta ingurunea [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman)-en eskuragarri.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
