# 📊 Versioning System Cleanup Report

## Analysis Completed

All documentation files (`.md`) have been analysed to identify which repositories are actually referenced.

## ✅ **KEPT Repositories (appear in documentation):**

| Repository | References | Main Use |
|-------------|-------------|-----------------|
| `dena_common_docs` | Multiple | Documentation and internal links |
| `dena_common_api` | 13 | Common API models |
| `dena_common_data_api` | Multiple | Data models (heavily used in links) |
| `dena_common_interop_api` | Multiple | Interoperability API (heavily used) |
| `dena_admin_conx_test` | Multiple | Connectivity test |
| `dena_interop_common_data_test` | Multiple | Common data tests |
| `dena_connector_base_rest` | 2 | Base REST connector |
| `dena_connector_demo1_rest` | 5 | Demo connector |
| `dena_devtools_services` | 5 | Development tools |
| `dena_config` | 46 | Configuration (heavily referenced) |

**Total kept:** 10 repositories

## ❌ **REMOVED Repositories (do NOT appear in documentation):**

| Repository | Reason |
|-------------|---------|
| `dena_interop_syncandretrieve_api` | Not referenced in docs |
| `dena_interopconfig_api` | Not referenced in docs |
| `dena_security_oauth_api` | Not referenced in docs |
| `dena_security_api` | Not referenced in docs |
| `dena_common_interop_core` | Not referenced in docs |
| `dena_security_oauth_core` | Not referenced in docs |
| `dena_interop_syncandretrieve_core` | Not referenced in docs |
| `dena_connector_archetype_rest` | Not referenced in docs |
| `dena_person_api` | Not referenced in docs |
| `dena_person_admin_sync_rest` | Not referenced in docs |
| `dena_config_byenv` | Not referenced in docs |
| `r01f_base` | Not referenced in docs |
| `r01f_core_services` | Not referenced in docs |
| `r01f_security` | Not referenced in docs |
| `r01f_business_services` | Not referenced in docs |

**Total removed:** 15 repositories

## 📈 **Improvement Achieved:**

- **Before:** 25 repositories in the versioning system
- **After:** 10 repositories actually in use
- **Reduction:** 60% fewer repositories to maintain
- **Benefit:** Cleaner and more maintainable system

## 🎯 **Repositories by Versioning Type:**

### With Specific Tags:
- `dena_admin_conx_test` → `v1.0.0`
- `dena_common_docs` → `v0.3.26`
- Rest → `v0.3.30-SNAPSHOT` or `v0.3.25-SNAPSHOT`

### With Specific Branches:
- `dena_common_data_api` → `develop`
- `dena_common_interop_api` → `develop`
- `dena_interop_common_data_test` → `develop`
- `dena_common_docs` → `main`

## ✨ **Final Optimised System:**

The `mkdocs-vars.yaml` file now contains only:
- ✅ Repositories that are actually used in the documentation
- ✅ Correctly versioned URLs
- ✅ Appropriate branches for each repository
- ✅ Relevant Maven commands and artefacts

## 🚀 **Next Step:**

Apply these variables to the existing documentation files that have hardcoded URLs.

---
**Date:** {{ dena.date }}  
**Version:** {{ dena.version }}  
**Maintainer:** DENA Versioning System
