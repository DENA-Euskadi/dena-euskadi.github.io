# :material-sync: METADATA-SYNC

> **Bertsioa:** `v{{ dena.version }}` · **Data:** {{ dena.date }}

---

## Zer da?

**Metadata-Sync** administrazioek DENAri jakinarazteko mekanismoa da, erabiltzaile bati lotutako edozein datutan aldaketak gertatzen direnean.

``` mermaid
sequenceDiagram
    participant Admin as Administrazioa
    participant DENA as CORE DENA
    participant App as Bezero-aplikazioa

    Admin->>DENA: POST /api/admin/interop/sync/metadata (X pertsonak aldaketak ditu)
    DENA-->>Admin: 200 OK

    Note over DENA: Metadatua gordetzen du: pertsona + mota + data

    App->>DENA: Berrikuntzarik al dago?
    DENA-->>App: Bai, Admin X-ek datu berriak ditu zuretzat
```

!!! info "Metadatuak soilik"

    DENAk **ez ditu datuak berak gordetzen**, azken eguneratzearen data bakarrik, konbinazio honen arabera:

    - Pertsona
    - Datu mota
    - Administrazioa

    Bezero-aplikazioak benetako datuak behar dituenean, [Data-Retrieve](../data-retrieve/index.md) bidez eskatuko ditu.

---

## Nola detektatu aldaketak zure administrazioan

Datu-jatorri batek integratzeko egin behar duen lehen gauza **zer aldatu den detektatzea** da. DENArentzat, behar den informazio bakarra hau da:

- **Zein pertsonak** duen daturen bat aldatuta (datu-motaka).
- **Noiz** izan zen azken aldaketa.

**Aldatu den datu zehatzak ez du axola**: datu-jatorrian aldaketa bat gertatu dela eta noiz gertatu den soilik axola du. Errenkada bat gehitzea edo ezabatzea ere aldaketatzat hartzen da.

Normalean SQL kontsulta sinple bat da datu-mota bakoitzeko. Adibidez, egitura hau duen negozio-taula baterako:

| PERSON_ID | PERSON_DATA | CREATED_AT | LAST_UPDATED_AT |
|-----------|-------------|------------|-----------------|
| 48291038Z | … | 2026-08-17T03:22:10Z | 2026-08-17T09:14:22Z |
| 10593847H | … | 2026-08-17T14:05:49Z | 2026-08-17T18:41:03Z |

data jakin baten ondorengo aldaketak dituzten pertsonak itzultzen dituen kontsulta zuzena da:

```sql
SELECT PERSON_ID,
       MAX(COALESCE(LAST_UPDATED_AT, CREATED_AT)) AS LAST_CHANGE_AT
  FROM DB_TABLE
 WHERE COALESCE(LAST_UPDATED_AT, CREATED_AT) >= :from
 GROUP BY PERSON_ID;
```

`COALESCE(LAST_UPDATED_AT, CREATED_AT)`-k `LAST_UPDATED_AT` erabiltzen du existitzen bada, eta `CREATED_AT`-ra jotzen du `NULL` bada. Emaitza (pertsona + azken aldaketaren unea) DENAra bidaltzen den SRMD item bakoitza bihurtzen dena da (`aboutPerson` + `someDataWasUpdatedAt` + `ofType`).

!!! tip "Biltzaile zentralizatua"

    Zure administrazioak datu-jatorri asko baditu, jatorri bakoitzeko osagai bat izan beharrean, **aldaketa-biltzaile zentralizatu** bat zabal dezakezu. Ikus [Integrazio-erreferentziako arkitekturak](../../arquitectura/arquitecturas-referencia.md).

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
