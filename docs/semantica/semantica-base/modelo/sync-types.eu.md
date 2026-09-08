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
    "admin": { "id": "EJGV", "oid": "..." },
    "aboutPerson": { "id": "11111111H", "oid": "..." },
    "someDataWasUpdatedAt": "2025-07-16T13:00:00.000Z",
    "ofType": { "id": "administrativeServiceProcedureRecord", "oid": "..." },
    "fromDataOriginInstance": "EJGV-Expedientes"
  },
  {
    "admin": { "id": "DFB", "oid": "..." },
    "aboutPerson": { "id": "11223344L", "oid": "..." },
    "someDataWasUpdatedAt": "2025-07-15T11:00:00.000Z",
    "ofType": { "id": "oneOffPayment", "oid": "..." },
    "fromDataOriginInstance": "DFB-Pagos"
  }
]
```

!!! info "Erabilera"
    DENA-CORE-k `someDataWasUpdatedAt` konparatzen du datu mota hori jatorritik azken aldiz berreskuratu zen unearekin, UIan erakusten den NEW/UPDATED/UNCHANGED egoera erabakitzeko.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
