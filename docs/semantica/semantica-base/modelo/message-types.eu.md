# :material-message-text: Mezu Motak

Orri honek DENAren **interop message**-aren egituran erabiltzen diren motak dokumentatzen ditu (`DN00InteropContext` eta `DN00InteropMessageData`).

---

## DN00InteropFlowDirection

Mezuaren norabidea adierazten duen Enum-a. Ez da modu independentean gordetzen: mezu motatik eratortzen da.

| Balioa | Deskribapena |
|---|---|
| `REQUEST` | Eskaera |
| `RESPONSE` | Eskaera baten erantzuna |

---

## DN00InteropMessageType

Elkarreragingarritasun-operazio mota identifikatzen duen Enum-a. Hauek dira enum-aren benetako balioak:

| Balioa | Esparrua |
|---|---|
| `CLIENT_LOGIN`, `CLIENT_LOGIN_DEMO` | Bezeroaren login-a |
| `CLIENT_PASSKEY_REGISTER_INIT` / `CLIENT_PASSKEY_REGISTER_FINISH` | Passkey erregistroa |
| `CLIENT_PASSKEY_LOGIN_INIT` / `CLIENT_PASSKEY_LOGIN_FINISH` | Passkey bidezko login-a |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS` | Passkey kredentzialen garbiketa |
| `CLIENT_INIT_REQ` / `CLIENT_INIT_RESP` | Bezeroaren hasieratzea |
| `CLIENT_SRMD_SYNC_REQ` / `CLIENT_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) bezerotik |
| `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) administraziotik |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Data-Retrieve bezerotik |
| `PERSON_FETCH_DATA` | Pertsona baten datuak berreskuratzea |
| `ADMIN_PERSON_PULL_BESPOKE_CREATE_REQ` | Person-Sync Pull: bespoke job-a sortzea |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH` | Person-Sync Pull: bespoke job-a kontsultatzea |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH` | Bespoke asset-aren deskarga |
| `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH` | Aurrez sortutako asset-aren deskarga |
| `PERSON_ADMIN_SEARCH` | Pertsonen bilaketa administrazioak eginda |
| `PERSON_ADMIN_HEAD` | Pertsonaren HEAD kontsulta administrazioak eginda |

---

## DN00IteropRouteDataItem

Mezu bat DENA osagai batetik igarotzen den bakoitzean, `interopRouteData` array-an **aztarna bat** uzten du. Auditoria eta arazketa helbururako erabiltzen da.

### Atributuak

| Eremua | Mota | Deskribapena |
|---|---|---|
| `denaComponentId` | `DN00InteropComponent` | DENA osagaiaren identifikatzailea |
| `timestamp` | `Instant` (ISO 8601) | Mezua osagaitik igaro zen unea |

### Osagai identifikatzaileak

| Balioa | Osagaia |
|---|---|
| `CLIENT_INSTALLMENT` | Bezeroaren instalazioa (DENA-APP) |
| `DENA_CORE` | DENA-CORE (modulu zentrala) |
| `DENA_ADMIN_CONNECTOR` | Administrazioaren konektorea |
| `ADMIN` | Administrazioaren sistema |

### Adibidea

```json
{
  "interopRouteData": [
    {
      "denaComponentId": "CLIENT_INSTALLMENT",
      "timestamp": "2026-08-18T11:28:47.523Z"
    },
    {
      "denaComponentId": "DENA_CORE",
      "timestamp": "2026-08-18T11:28:47.601Z"
    },
    {
      "denaComponentId": "DENA_ADMIN_CONNECTOR",
      "timestamp": "2026-08-18T11:28:47.688Z"
    }
  ]
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
