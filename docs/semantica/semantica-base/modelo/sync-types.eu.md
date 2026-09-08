# :material-sync: Sync Fluxuaren Datu Motak

Orri honek administrazioen eta DENA-CORE-ren arteko **metadatuen sinkronizazio fluxuan** (Metadata-Sync / SRMD) erabiltzen diren datu-egiturak dokumentatzen ditu.

---

## DenaInteroperableDataTypeSync

Datu-iturri / administrazio batek **datu mota elkarreragile baten meta-data sync-a** bidaltzeko erabiltzen duen egitura, DENAn erregistratutako pertsonentzako datuetako aldaketak, gehiketak edo ezabaketak informatuz.

### Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `interoperableDataTypeId` | `ID` | :material-check: | Datu motaren identifikatzailea (adib: `PAYMENTS`, `RECORDS`) |
| `personDataSync` | `Array(DenaPersonDataSync)` | :material-check: | Pertsona bakoitzeko hautemandako aldaketen array-a |

### Adibidea

```json
{
  "interoperableDataTypeId": "RECORDS",
  "personDataSync": [
    {
      "personId": "11111111H",
      "lastUpdated": "2025-07-16T13:00:00.000Z",
      "change": "CHANGED"
    },
    {
      "personId": "11223344L",
      "lastUpdated": "2025-07-15T11:00:00.000Z",
      "change": "NEW"
    }
  ]
}
```

---

## DenaPersonDataSync

Datu mota elkarreragile baterako **pertsona baten** datuetako aldaketak informatzen dituen egitura.

### Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `personId` | `ID` | :material-check: | Pertsonaren identifikatzaile administratiboa (NAN/IFK/AIZ/Pasaportea) |
| `objectOid` | `OID` | :material-close: | DENAren pertsona-moduluko objektuaren identifikatzaile bakarra |
| `lastUpdated` | `UTCDateTime` | :material-check: | Datuaren azken eguneratzearen data/ordua |
| `change` | `Enum` | :material-check: | Hautemandako aldaketa mota |

### `change` balioak

| Balioa | Deskribapena |
|---|---|
| `CHANGED` | Lehendik dagoen erregistro bat aldatu da |
| `NEW` | Erregistro berri bat sortu da |
| `DELETED` | Erregistro bat ezabatu da |

---

## DenaPersonMetaDataSyncItem

UIak (DENA-APP) **pertsona baten** meta-data sync-a eskatzeko erabiltzen duen egitura, datu-iturri guztietako datu mota elkarreragile guztien gainean.

### Atributuak

| Eremua | Mota | Derrigorrezkoa | Deskribapena |
|---|---|:---:|---|
| `adminId` | `ID` | :material-check: | DENAko administrazioaren identifikatzailea |
| `dataSourceId` | `ID` | :material-check: | DENAko datu-iturriaren identifikatzailea |
| `dataTypeId` | `ID` | :material-check: | DENAko datu motaren identifikatzailea |
| `lastUpdateTS` | `TimeStamp` | :material-check: | Iturriko datu motaren edozein erregistroren azken aldaketaren data |

### Adibidea

```json
[
  {
    "adminId": "EJGV",
    "dataSourceId": "EJGV-Expedientes",
    "dataTypeId": "RECORDS",
    "lastUpdateTS": 1670374400
  },
  {
    "adminId": "DFB",
    "dataSourceId": "DFB-Pagos",
    "dataTypeId": "PAYMENTS",
    "lastUpdateTS": 1670380000
  }
]
```

!!! info "UIan erabilera"
    UIak `lastUpdateTS` erabiltzen du iturriko datuak RETRIEVE egin behar dituen kalkulatzeko. Retrieve egiteko, UIak DENA-CORE-ko retrieve URLa ezagutu behar du datu-iturri bakoitzerako (datu-iturrietako erregistroan eskuragarri).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
