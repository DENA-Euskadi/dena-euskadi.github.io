# Servicio Administrativo (Administrative Service)

> - **Versión:** `v0.3.26`
> - **Fecha:** 2026-06-11
> - **Test:** [DN99DENATestMockObjFactoryForAdmistrativeServiceObjectBase.java](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceObjectBase.java)
> - **Código:**
>   - [DN00AdmistrativeService.java](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeService.java)
>   - [DN00AdministrativeServiceProcedure.java](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedure.java)

## Descripción

El **Servicio** y el **Procedimiento** forman el contexto jerárquico del expediente. Todo expediente pertenece a un procedimiento, y todo procedimiento pertenece a un servicio.

```mermaid
---
config:
  theme: base
  themeVariables:
    primaryColor: "#FFFFFF"
    primaryTextColor: "#000000"
    primaryBorderColor: "#CCCCCC"
    lineColor: "#4A4A4A"
    fontSize: "12px"
---
flowchart TD
    SRV["<b>Servicio</b><br/><i>serviceNameByLanguage<br/>originRef · DENARef · SIARef</i>"]
    PROC["<b>Procedimiento</b><br/><i>serviceNameByLanguage<br/>originRef · DENARef · SIARef</i>"]
    EXP["<b>Expediente</b> (1:N)"]

    SRV --> PROC
    PROC --> EXP

    EXP -.-> NOT["Notificaciones"]
    EXP -.-> REG["Registros oficiales"]
    EXP -.-> PAY["Pagos"]

    SRV --> ORG["participatingOrgUnits[]<br/><i>orgUnit · role</i>"]
    SRV --> REFS["originRef / DENARef / SIARef<br/><i>oid · id · urls</i>"]

    style SRV fill:#f5f5f5,stroke:#666666,color:#000000,rx:8,ry:8
    style PROC fill:#f5f5f5,stroke:#666666,color:#000000,rx:8,ry:8
    style EXP fill:#ffe6cc,stroke:#d79b00,color:#000000,rx:8,ry:8
    style NOT fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style REG fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style PAY fill:#fff2cc,stroke:#d6b656,color:#000000,rx:6,ry:6
    style ORG fill:#d5e8d4,stroke:#82b366,color:#000000,rx:6,ry:6
    style REFS fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6

    click EXP "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/data-retrieve/data/expediente.md" "Ver Expediente"
    click NOT "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/data-retrieve/data/notificacion.md" "Ver Notificación"
    click REG "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/data-retrieve/data/registro-oficial.md" "Ver Registro Oficial"
    click PAY "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/data-retrieve/data/pago.md" "Ver Pago"
    click ORG "https://github.com/DENA-Euskadi/dena-common-docs/blob/main/docs/semantica/data-retrieve/data/unidad-organica.md" "Ver Unidad Orgánica"
```

| Color | Significado |
|-------|-------------|
| ⚪ Gris | Contexto jerárquico (Servicio / Procedimiento) |
| 🟠 Naranja | Objeto de dominio (Expediente) |
| 🟡 Amarillo | Objetos dependientes (Notificación, Registro, Pago) |
| 🟢 Verde | Unidades orgánicas |
| 🔵 Azul claro | Referencias (originRef / DENARef / SIARef) |

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00AdmistrativeService | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeService.java) |
| DN00AdministrativeServiceProcedure | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdministrativeServiceProcedure.java) |
| DN00AdmistrativeServiceObjectBase | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/DN00AdmistrativeServiceObjectBase.java) |
| DN00AdministrativeServiceReference | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/refs/DN00AdministrativeServiceReference.java) |
| DN00AdministrativeServiceProcedureReference | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/refs/DN00AdministrativeServiceProcedureReference.java) |
| DN00AdministrativeServiceObjectReferenceBase | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/refs/DN00AdministrativeServiceObjectReferenceBase.java) |

---

## 🧪 Tests y ejemplos

> Repositorio: [DENA-Euskadi/dena-interop-common-data-test](https://github.com/DENA-Euskadi/dena-interop-common-data-test)

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForAdmistrativeServiceObjectBase | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeServiceObjectBase.java) |
| DN99DENATestMockObjFactoryForAdmistrativeService | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdmistrativeService.java) |
| DN99DENATestMockObjFactoryForAdministrativeServiceProcedure | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdministrativeServiceProcedure.java) |
| DN99DENATestMockObjFactoryForAdministrativeServiceReference | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdministrativeServiceReference.java) |
| DN99DENATestMockObjFactoryForAdministrativeServiceProcedureReference | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/services/DN99DENATestMockObjFactoryForAdministrativeServiceProcedureReference.java) |

---

## Servicio — Atributos JSON


| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `serviceNameByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Licencias de actividad"}` | Nombre del servicio |
| `serviceUrls` | `Array` | ❌ | | URLs del catálogo de servicios |
| `participatingOrgUnits` | `Array` | ❌ | *(ver [unidad-organica.md](./unidad-organica.md) · [`DN00OrgUnitReferenceWithRole`](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/org/DN00OrgUnitReferenceWithRole.java))* | Unidades orgánicas participantes |
| `originRef` | `Object` | ✅ | *(ver [`DN00AdministrativeServiceReference`](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/refs/DN00AdministrativeServiceReference.java))* | Referencia en la administración de origen |
| `originRef.oid` | `String` | ❌ | `"SRV-OID-001"` | OID en el catálogo de origen |
| `originRef.id` | `String` | ✅ | `"SRV-LIC-ACT"` | ID en el catálogo de origen |
| `originRef.urls` | `Array` | ❌ | | URLs en el catálogo de origen |
| `DENARef` | `Object` | ❌ | | Referencia en el catálogo DENA (si existe) |
| `DENARef.oid` | `String` | ❌ | `"DENA-SRV-001"` | OID en el catálogo DENA |
| `DENARef.id` | `String` | ❌ | `"DENA-LIC-001"` | ID en el catálogo DENA |
| `SIARef` | `Object` | ❌ | | Referencia en el catálogo SIA de la AGE (si existe) |
| `SIARef.oid` | `String` | ❌ | `"SIA-001"` | OID en el catálogo SIA |
| `SIARef.id` | `String` | ❌ | `"SIA-LIC-001"` | ID en el catálogo SIA |

---

## Procedimiento — Atributos JSON


| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `serviceNameByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Solicitud licencia"}` | Nombre del procedimiento |
| `serviceUrls` | `Array` | ❌ | | URLs del catálogo |
| `participatingOrgUnits` | `Array` | ❌ | *(ver [unidad-organica.md](./unidad-organica.md) · [`DN00OrgUnitReferenceWithRole`](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/org/DN00OrgUnitReferenceWithRole.java))* | Unidades orgánicas participantes |
| `originRef` | `Object` | ✅ | *(ver [`DN00AdministrativeServiceProcedureReference`](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIAdministrativeServicesModelClasses/src/main/java/dena/api/data/model/administrativeservices/refs/DN00AdministrativeServiceProcedureReference.java))* | Referencia en la administración de origen |
| `originRef.oid` | `String` | ❌ | `"PROC-OID-001"` | OID en el catálogo de origen |
| `originRef.id` | `String` | ✅ | `"PROC-LIC-APER"` | ID en el catálogo de origen |
| `originRef.urls` | `Array` | ❌ | | URLs en el catálogo de origen |
| `DENARef` | `Object` | ❌ | | Referencia en el catálogo DENA (si existe) |
| `SIARef` | `Object` | ❌ | | Referencia en el catálogo SIA (si existe) |

---

## Estructura de las referencias (`originRef`, `DENARef`, `SIARef`)

Todas las referencias comparten la misma estructura:

```json
{
  "oid": "SRV-OID-001",
  "id": "SRV-LIC-ACT",
  "urls": [
    { "url": "https://catalogo.miadmin.eus/servicio/SRV-LIC-ACT", "language": "SPANISH" }
  ]
}
```

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `oid` | `String` | Identificador técnico en el catálogo |
| `id` | `String` | Identificador de negocio en el catálogo |
| `urls` | `Array` | URLs de acceso al catálogo |

---

## Ejemplo JSON (dentro de un expediente)

```json
{
  "service": {
    "serviceNameByLanguage": { "SPANISH": "Licencias de actividad", "BASQUE": "Jarduera-lizentziak" },
    "originRef": { "oid": "SRV-OID-001", "id": "SRV-LIC-ACT" },
    "DENARef": null,
    "SIARef": null,
    "participatingOrgUnits": [
      {
        "orgUnit": { "id": "ORG-URBANISMO", "displayNameByLanguage": { "SPANISH": "Departamento de Urbanismo" } },
        "role": "RESPONSIBLE"
      }
    ]
  },
  "procedure": {
    "serviceNameByLanguage": { "SPANISH": "Solicitud de licencia de apertura", "BASQUE": "Irekiera-lizentzia eskaera" },
    "originRef": { "oid": "PROC-OID-001", "id": "PROC-LIC-APER" },
    "DENARef": null,
    "SIARef": null
  }
}
```

---

## Jerarquía

```
Servicio Administrativo
 └── Procedimiento
      └── Expediente (1:N)
           ├── Notificaciones
           ├── Registros oficiales
           └── Pagos
```

> **Nota:** Las citas (`scheduleItem`) NO pertenecen a esta jerarquía. Son objetos independientes.

---

## Notas para la administración

- `originRef.id` es obligatorio: la administración debe proporcionar al menos el identificador de negocio del servicio y procedimiento en su catálogo propio.
- `DENARef` y `SIARef` son opcionales. Si la administración conoce el código DENA o SIA, puede incluirlo para facilitar la correlación.
- `serviceNameByLanguage` debe incluir al menos textos en `SPANISH` y `BASQUE`.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
