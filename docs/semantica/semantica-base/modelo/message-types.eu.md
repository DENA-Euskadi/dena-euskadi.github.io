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

Elkarreragingarritasun-operazio mota identifikatzen duen Enum-a. `DN00InteropMessageType` enum-aren balio guztien zerrenda norabidearekin eta parte hartzen duten aleekin (iturria: `DN00InteropMessageType.java`):

| Eskaera (`_REQ`) / Erantzuna (`_RESP`) | Eragiketaren izena | Jatorria → Helburua |
|---|---|---|
| `CLIENT_LOGIN_DEMO` | Bezeroaren demo login-a (ez du bikoteko erantzunik) | CLIENT_INSTALLMENT → DENA_CORE |
| `CLIENT_LOGIN_REQ` / `CLIENT_LOGIN_RESP` | Bezeroaren login-a | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_REFRESH_TOKEN_REQ` / `CLIENT_REFRESH_TOKEN_RESP` | Token-aren berrikuntza | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_CLEAN_CREDENTIALS_REQ` / `CLIENT_PASSKEY_CLEAN_CREDENTIALS_RESP` | Passkey kredentzialen garbiketa | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_LOGIN_INIT_REQ` / `CLIENT_PASSKEY_LOGIN_INIT_RESP` | Passkey bidezko login-a — hasiera | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_LOGIN_FINISH_REQ` / `CLIENT_PASSKEY_LOGIN_FINISH_RESP` | Passkey bidezko login-a — amaiera | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_REGISTER_INIT_REQ` / `CLIENT_PASSKEY_REGISTER_INIT_RESP` | Passkey erregistroa — hasiera | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PASSKEY_REGISTER_FINISH_REQ` / `CLIENT_PASSKEY_REGISTER_FINISH_RESP` | Passkey erregistroa — amaiera | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_LOGIN_REQ` / `ADMIN_LOGIN_RESP` | Administrazioaren login-a | ADMIN ↔ DENA_CORE |
| `CLIENT_INIT_REQ` / `CLIENT_INIT_RESP` | Bezeroaren hasieratzea | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_SRMD_SYNC_REQ` / `CLIENT_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) bezerotik abiatua | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_SRMD_SYNC_REQ` / `ADMIN_SRMD_SYNC_RESP` | Metadata-Sync (SRMD) administraziotik abiatua | ADMIN ↔ DENA_CORE |
| `CLIENT_RETRIEVE_REQ` / `CLIENT_RETRIEVE_RESP` | Data-Retrieve: datuak bezerotik berreskuratzea | CLIENT_INSTALLMENT ↔ ADMIN |
| `CLIENT_USER_DELETE_REQ` / `CLIENT_USER_DELETE_RESP` | Erabiltzailearen ezabaketa bezerotik | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_USER_DELETE_BY_ID_REQ` / `CLIENT_USER_DELETE_BY_ID_RESP` | Erabiltzailearen ezabaketa IDaren bidez bezerotik | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `PERSON_ADMIN_SEARCH_REQ` / `PERSON_ADMIN_SEARCH_RESP` | Pertsonen bilaketa administrazioak eginda | ADMIN ↔ DENA_CORE |
| `PERSON_FETCH_DATA_REQ` / `PERSON_FETCH_DATA_RESP` | Pertsona baten datuak berreskuratzea | ADMIN ↔ DENA_CORE |
| `PERSON_ADMIN_HEAD_REQ` / `PERSON_ADMIN_HEAD_RESP` | Pertsonaren HEAD kontsulta administrazioak eginda | ADMIN ↔ DENA_CORE |
| `PERSON_PUSH_TO_ADMIN_REQ` / `PERSON_PUSH_TO_ADMIN_RESP` | Person-Sync Push: DENAk pertsona-datuak administraziora bidaltzen ditu | DENA_CORE ↔ ADMIN |
| `CLIENT_PERSON_FETCH_REQ` / `CLIENT_PERSON_FETCH_RESP` | Pertsona-datuen kontsulta bezero-gailutik | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `CLIENT_PERSON_UPDATE_REQ` / `CLIENT_PERSON_UPDATE_RESP` | Pertsona-datuen eguneratzea bezero-gailutik | CLIENT_INSTALLMENT ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_BESPOKE_CREATE_REQ` / `ADMIN_PERSON_PULL_BESPOKE_CREATE_RESP` | Person-Sync Pull: bespoke job-a sortzea | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_BESPOKE_FETCH_REQ` / `ADMIN_PERSON_PULL_BESPOKE_FETCH_RESP` | Person-Sync Pull: bespoke job-aren emaitza kontsultatzea | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PULL_PREGEN_FETCH_REQ` / `ADMIN_PERSON_PULL_PREGEN_FETCH_RESP` | Person-Sync Pull: aurrez sortutako job-aren emaitza kontsultatzea | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_BESPOKE_EXPORT_ASSET_FETCH_RESP` | Person-Sync Pull: bespoke asset-aren deskarga | ADMIN ↔ DENA_CORE |
| `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_REQ` / `ADMIN_PERSON_PREGEN_EXPORT_ASSET_FETCH_RESP` | Person-Sync Pull: aurrez sortutako asset-aren deskarga | ADMIN ↔ DENA_CORE |

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
| `DENA_INTEROP_ADMIN_SYNC` | Administrazioaren interop sinkronizazio osagaia |
| `ADMIN` | Administrazioaren sistema |
| `DENA_PERSON_ADMIN` | DENAren pertsona-administrazio modulua |
| `DENA_PERSON_ADMIN_SYNC` | DENAren pertsona-sinkronizazio zerbitzua |
| `DENA_SANDBOX_ADMIN` | DENAren sandbox administrazioa (proba-inguruneak) |

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
