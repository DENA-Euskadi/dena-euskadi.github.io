# :material-upload: PERSON-SYNC — PUSH

**Push** mekanismoarekin, DENAk zure administrazioari **proaktiboki** jakinarazten dio pertsona bat erregistratzen den, bere datuak aldatzen dituen edo kontua ezabatzen duen bakoitzean. Aldaketei **denbora errealean** erantzun behar diezunean gomendatzen den mekanismoa da.

---

## Nola funtzionatzen du?

Push-en, **DENA-CORE da bezeroa** eta **zure administrazioa zerbitzaria**: DENAk HTTP `POST` bat egiten dio zuk erakusten duzun endpoint bati, aldaketaren datuekin.

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (bezeroa)
    participant Admin as Zure administrazioa (zerbitzaria)

    Note over DENA: Pertsona erregistratu / aldatu / ezabatu da
    DENA->>Admin: POST <konfiguratutako-url-a> (aldaketaren datuak + gertaera)
    Admin->>Admin: Gertaeraren arabera prozesatu (sortu / eguneratu / ezabatu)
    Admin-->>DENA: 200 OK
```

!!! important "DENAk ez du bide finkorik ezartzen"

    Ez dago push endpointerako aurrez definitutako biderik. Zure administrazioak DENAn bere konektorerako **konfiguratu duen URLan** erakusten du. DENAk URL horretara egingo du `POST`. Ezinbestekoa den gauza bakarra da `POST` bat onartzea `application/json`-ekin eta HTTP kode zuzena itzultzea.

---

## Zer inplementatu behar du administrazioak?

!!! info "Aldaketa jaso eta gertaeraren arabera jokatzen duen endpoint bat"

    1. **`POST` REST endpoint bat erakutsi** jakinarazpenaren JSON gorputza jasotzeko.
    2. **`syncEvent` eremua begiratu** zer gertatu den jakiteko eta horren arabera jokatzeko:

    | Gertaera | Ekintza |
    |----------|---------|
    | `CREATED` | Pertsona zure kopia lokalean altan eman |
    | `UPDATED` | Pertsonaren datuak eguneratu |
    | `ID_CHANGED` | NANa/AIZ eguneratu (`oid`-aren bidez kokatuta) |
    | `DELETED` | Pertsona **eta harekin lotutako datuak** ezabatu |

    3. **Emaitza errealari dagokion HTTP kodearekin erantzun** (`200` dena ondo joan bada).

---

## Zergatik da garrantzitsua kopia hori mantentzea?

Zure administrazioak DENAn **benetan kontua duten** pertsonen aldaketa-jakinarazpenak ([Metadata-Sync / SRMD](../metadata-sync/index.md)) baino ez lituzke bidali behar. Push-ek zure kopia lokala egunean mantentzen du, honela:

- Ez dituzu SRMDak bidaltzen DENAn ez dauden pertsonentzat.
- Kontua ezabatu duten pertsonei SRMDak bidaltzeari uzten diozu (`DELETED` gertaera).
- Pertsona berrien oinarrizko datuak (izena, kontaktua) berehala izaten dituzu.

---

## Endpointaren kontratua

Espezifikazio osoa —eskaeraren gorputza, gertaeraka prozesatzea, erantzuna, HTTP kodeak eta inplementazio-egiaztapena— hemen dago:

[:octicons-arrow-right-24: Endpoint Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md)

---

!!! tip "Noiz erabili Push"

    - Pertsonen aldaketei **denbora errealean** erantzun behar diezunean.
    - Aldizkako fitxategien mende egon nahi ez duzunean ([Pull](./pull.md)).
    - Zure sistemak datua berehala behar duenean Metadata-Sync bidez aldaketak jakinarazteko.

    !!! note "Gomendioa: Push + Pull"
        **Bi** mekanismoak inplementatzea gomendatzen da: Push denbora errealerako eta [Pull off-line](./pull.md) galdutako jakinarazpenak berreskuratzeko babes gisa.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
