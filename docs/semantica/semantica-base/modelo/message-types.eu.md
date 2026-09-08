# :material-message-text: Mezu Motak

Orri honek DENAren elkarreragingarritasun-mezuen egituran barnean erabiltzen diren datu motak dokumentatzen ditu.

---

## DenaFlowDirection

Elkarrizketan mezuaren norabidea adierazten duen Enum-a.

| Balioa | Deskribapena |
|---|---|
| `REQ` | Eskaera (request) |
| `RES` | Eskaera baten erantzuna (response) |

---

## DenaMessageType

Elkarreragingarritasun-operazio mota identifikatzen duen Enum-a. Fluxu bakoitzak bere mezu motak ditu:

| Fluxua | Jatorria | Helmuga | Balioa | Deskribapena |
|---|---|---|---|---|
| Person-Sync | DENA-CORE | Admin | `DENA_USER_SYNC_PUSH` | Pertsonen alta/baja jakinarazpena |
| Metadata-Sync | Client | DENA-CORE | `UI_META_DATA_SYNC` | UIren metadatuen sinkronizazioa |
| Metadata-Sync | Admin | DENA-CORE | `ADMIN_SYNC_PUSH` | Administraziotik bidalitako aldaketak (PUSH) |
| Metadata-Sync | DENA-CORE | Admin | `DENA_SYNC_PULL` | Administrazioari aldaketak eskatzea (PULL) |
| Data-Retrieve | Client | DENA-CORE | `UI_DATA_RETRIEVE` | UItik datuak eskatzea |
| Data-Retrieve | DENA-CORE | Admin | `DENA_DATA_RETRIEVE` | Administrazioari datuak eskatzea |

---

## DenaInteropRouteDataItem

Elkarreragingarritasun-mezu bat DENA osagai batetik igarotzen den bakoitzean, `interopRouteData` array-an **aztarna bat** uzten du. Informazio hau batez ere auditoria eta arazketa helbururako erabiltzen da.

### Atributuak

| Eremua | Mota | Deskribapena |
|---|---|---|
| `denaComponentId` | `ID` | DENA osagaiaren identifikatzailea |
| `timeStamp` | `TimeStamp` | Mezua osagaitik igaro zen unea |

### Osagai identifikatzaile ezagunak

| ID | Osagaia |
|---|---|
| `mobileApp` | DENA app mugikorra |
| `webApp` | DENA web aplikazioa |
| `apiGateway` | API Gateway |
| `denaCORE` | DENA-CORE (modulu zentrala) |
| `connector` | Administrazioaren konektorea |

### Adibidea

```json
{
  "interopRouteData": [
    {
      "denaComponentId": "mobileApp",
      "timeStamp": 1670374400
    },
    {
      "denaComponentId": "denaCORE",
      "timeStamp": 1670374401
    },
    {
      "denaComponentId": "connector",
      "timeStamp": 1670374402
    }
  ]
}
```

---

## DenaPersonAndConsentGiven

Pertsonaren gutxieneko datuak emandako baimen baten datuekin konbinatzen dituen egitura.

| Eremua | Mota | Deskribapena |
|---|---|---|
| `personRef` | [DenaPersonRef](./person-ref.md) | Pertsonaren gutxieneko datuak (oid, id, alta-data, etab.) |
| `consentRef` | `DenaConsentRef` | Baimenaren erreferentzia (oid, url, etab.) |

### Adibidea

```json
{
  "personRef": {
    "personId": "12345678A",
    "objectOid": "6AE83A0C-2202-4666-9857-3334C14663A2"
  },
  "consentRef": {
    "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentURL": "https://interop.api.dena.eus/consent/db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
  }
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
