# :material-account-sync: PERSON-SYNC

> **Bertsioa:** `v{{ dena.version }}` · **Data:** {{ dena.date }}

---

## Zer da?

**Person-Sync**-ek administrazioei DENA-n erregistratutako pertsonak beren sistemetan sinkronizatzeko aukera ematen die, pertsona horiei lotutako datuetako eguneraketak DENA-ri jakinarazi ahal izateko.

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
    DENA[CORE DENA] -->|Push: aldaketak jakinarazten ditu| Admin[Administrazioa]
    Admin -->|Pull: pertsonak deskargatzen ditu| DENA
    
    style DENA fill:#70d680,stroke:#1a4d1f,color:#1a4d1f,stroke-width:3px
    style Admin fill:#e3f2fd,stroke:#1565c0,color:#1565c0,stroke-width:2px
```

---

## Mekanismoak

=== ":material-download: Pull (Administrazioa → DENA)"

    Administrazioa DENA-ra konektatzen da eta pertsonen datuak deskargatzen ditu.

    - Aldaketa inkrementalekin aurrez sortutako fitxategiak (aldizka)
    - Iragazki pertsonalizatuekin neurri-fitxategiak eskatzeko aukera

    [:octicons-arrow-right-24: Pull dokumentazioa](./pull.md)

=== ":material-upload: Push (DENA → Administrazioa)"

    DENA-k modu proaktiboan jakinarazten dio administrazioari pertsona berri bat erregistratzen denean edo aldaketak gertatzen direnean.

    - Administrazioak jasotze-endpoint bat eskaintzen du
    - DENA-k jakinarazpena bidaltzen du aldaketaren unean

    [:octicons-arrow-right-24: Push dokumentazioa](./push.md)

---

## Endpoint-ak

### Pull

| Dokumentua | Edukia |
|---|---|
| [Fetch Persons Pregen Export Asset](./endpoints/pull/fetch-persons-pregen-export-asset.md) | Aurrez sortutako fitxategien deskarga |
| [Create Pull from Admin Bespoke Job](./endpoints/pull/create-pull-from-admin-bespoke-job.md) | Neurri-esportazio eskaera |
| [Get Pull from Admin Bespoke Job](./endpoints/pull/get-pull-from-admin-bespoke-job.md) | Eskaeren egoera-kontsulta |
| [Fetch Persons Bespoke Export Asset](./endpoints/pull/fetch-persons-bespoke-export-asset.md) | Neurri-fitxategien deskarga |

### Push

| Dokumentua | Edukia |
|---|---|
| [Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) | Jasotze-endpoint-aren kontratua |

---

!!! tip "Postman"

    Postman bilduma eta environment-a eskuragarri [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman) helbidean.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
