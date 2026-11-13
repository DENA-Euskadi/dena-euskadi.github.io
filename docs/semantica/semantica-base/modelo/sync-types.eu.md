# :material-sync: Sync Fluxuaren Datu Motak

Orri honek administrazio batek DENA-CORE-ri bidaltzen dion **metadatuen sinkronizazio-fluxuan** (Metadata-Sync / SRMD) erabiltzen den egitura dokumentatzen du.

---

## DN00SyncMetaDataFromAdminToCOREItem

Administrazio batek DENA-CORE-ri bidaltzen dion itema, jatorrian pertsona baten **datu bat eguneratu dela** jakinarazteko, datu mota jakin baterako. Item hauen array gisa bidaltzen da.

Klasea: `DN00SyncMetaDataFromAdminToCOREItem` (`@MarshallType(as="syncMetaDataFromAdminToCORE")`).

### JSON atributuak

| Eremua | Mota | Beharrezkoa | Deskribapena |
|---|---|:---:|---|
| `admin` | [OrgAdminRef](./org-admin-ref.md) | :material-check: | Jatorrizko administrazioa |
| `aboutPerson` | [PersonRef](./person-ref.md) | :material-check: | Datua zein pertsonari dagokion |
| `someDataWasUpdatedAt` | `Instant` | :material-check: | Datua administrazioan azken aldiz eguneratu zen unea |
| `ofType` | [DataTypeRef](./data-type-ref.md) | :material-check: | Datu mota |
| `fromDataOriginInstance` | `ID` | :material-close: | Datu-jatorriaren instantzia (zein kudeaketa-sistemak ematen duen datua) |
| `popMessageAfterSync` | `Object` | :material-close: | Sinkronizatu ondoren bezeroan erakusteko aukerako mezua (adib. "Espediente berri bat duzu XX-n") |

### Adibidea

```json
[
    {
        "admin": { "id": "SANDBOXCIF" },
        "aboutPerson": { "id": "11189396B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
        "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
        "ofType": { "id": "PERSON_DATA" }
    },
    {
        "admin": { "id": "SANDBOXCIF" },
        "aboutPerson": { "id": "11111111B", "oid": "631011F3-9A2F-4D08-9F56-B7F4D6A0749E" },
        "someDataWasUpdatedAt": "2026-11-09T20:41:39.8825952Z",
        "ofType": { "id": "PERSON_DATA" }
    }
]
```

!!! info "Erabilera"
    DENA-CORE-k `someDataWasUpdatedAt` konparatzen du datu mota hori jatorritik azken aldiz berreskuratu zen unearekin, UIan erakusten den NEW/UPDATED/UNCHANGED egoera erabakitzeko.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
