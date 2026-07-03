# 📊 Bertsioketa-sistemaren garbiketa-txostena

## Analisia osatuta

Dokumentazio-fitxategi guztiak (`.md`) aztertu dira benetan erreferentziatzen diren biltegiak identifikatzeko.

## ✅ **MANTENDUTAKO biltegiak (dokumentazioan agertzen direnak):**

| Biltegia | Erreferentziak | Erabilera nagusia |
|-------------|-------------|-----------------|
| `dena_common_docs` | Anitz | Dokumentazioa eta barne-estekak |
| `dena_common_api` | 13 | API eredu komunak |
| `dena_common_data_api` | Anitz | Datu-ereduak (esteketan asko erabilia) |
| `dena_common_interop_api` | Anitz | Elkarreragingarritasun API-a (asko erabilia) |
| `dena_admin_conx_test` | Anitz | Konektibitate-testa |
| `dena_interop_common_data_test` | Anitz | Datu komunen testak |
| `dena_connector_base_rest` | 2 | Oinarrizko REST konektorea |
| `dena_connector_demo1_rest` | 5 | Demo konektorea |
| `dena_devtools_services` | 5 | Garapen-tresnak |
| `dena_config` | 46 | Konfigurazioa (asko erreferentziatua) |

**Mantendutako guztira:** 10 biltegi

## ❌ **EZABATUTAKO biltegiak (dokumentazioan EZ agertzen direnak):**

| Biltegia | Arrazoia |
|-------------|---------|
| `dena_interop_syncandretrieve_api` | Ez erreferentziatua dokumentazioan |
| `dena_interopconfig_api` | Ez erreferentziatua dokumentazioan |
| `dena_security_oauth_api` | Ez erreferentziatua dokumentazioan |
| `dena_security_api` | Ez erreferentziatua dokumentazioan |
| `dena_common_interop_core` | Ez erreferentziatua dokumentazioan |
| `dena_security_oauth_core` | Ez erreferentziatua dokumentazioan |
| `dena_interop_syncandretrieve_core` | Ez erreferentziatua dokumentazioan |
| `dena_connector_archetype_rest` | Ez erreferentziatua dokumentazioan |
| `dena_person_api` | Ez erreferentziatua dokumentazioan |
| `dena_person_admin_sync_rest` | Ez erreferentziatua dokumentazioan |
| `dena_config_byenv` | Ez erreferentziatua dokumentazioan |
| `r01f_base` | Ez erreferentziatua dokumentazioan |
| `r01f_core_services` | Ez erreferentziatua dokumentazioan |
| `r01f_security` | Ez erreferentziatua dokumentazioan |
| `r01f_business_services` | Ez erreferentziatua dokumentazioan |

**Ezabatutako guztira:** 15 biltegi

## 📈 **Lortutako hobekuntza:**

- **Aurretik:** 25 biltegi bertsioketa-sisteman
- **Ondoren:** 10 biltegi benetan erabiltzen direnak
- **Murrizketa:** %60 gutxiago biltegi mantentzeko
- **Onura:** Sistema garbiagoa eta mantentzeko errazagoa

## 🎯 **Biltegiak bertsioketa motaren arabera:**

### Tag espezifikoekin:
- `dena_admin_conx_test` → `v1.0.0`
- `dena_common_docs` → `v0.3.26`
- Gainerakoak → `v0.3.30-SNAPSHOT` edo `v0.3.25-SNAPSHOT`

### Branch espezifikoekin:
- `dena_common_data_api` → `develop`
- `dena_common_interop_api` → `develop`
- `dena_interop_common_data_test` → `develop`
- `dena_common_docs` → `main`

## ✨ **Azken sistema optimizatua:**

`mkdocs-vars.yaml` fitxategiak orain soilik dauka:
- ✅ Dokumentazioan benetan erabiltzen diren biltegiak
- ✅ Behar bezala bertsiokatutako URLak
- ✅ Biltegi bakoitzarentzako branch egokiak
- ✅ Maven komando eta artefaktu garrantzitsuak

## 🚀 **Hurrengo urratsa:**

Aldagai hauek URL hardcodeatu dituzten dokumentazio-fitxategi existenteetan aplikatzea.

---
**Data:** {{ dena.date }}  
**Bertsioa:** {{ dena.version }}  
**Mantendatzailea:** DENA Bertsioketa-sistema
