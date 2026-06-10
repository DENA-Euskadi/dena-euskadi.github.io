# Unidad Orgánica (Org Unit)

> - **Versión:** `v0.3.26`
> - **Fecha:** 2026-06-11
> - **Test:** [DN99DENATestMockObjFactoryForOrgUnitReference.java](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/DN99DENATestMockObjFactoryForOrgUnitReference.java)
> - **Código:** [DN00OrgUnitReference.java](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/org/DN00OrgUnitReference.java)

## Descripción

Representa una unidad organizativa de la administración que participa en un servicio o procedimiento con un rol determinado.

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
flowchart LR
    UNIT["<b>Unidad Orgánica</b><br/><i>orgUnitReferenceWithRole</i>"]

    UNIT --> ORG["orgUnit<br/><i>oid · id · dir3Id<br/>displayNameByLanguage<br/>urls</i>"]
    UNIT --> ROLE["role<br/><i>RESPONSIBLE · MANAGING<br/>INFORMER · SOLVER · OTHER</i>"]

    style UNIT fill:#d5e8d4,stroke:#82b366,color:#000000,rx:8,ry:8
    style ORG fill:#dae8fc,stroke:#6c8ebf,color:#000000,rx:6,ry:6
    style ROLE fill:#e1d5e7,stroke:#9673a6,color:#000000,rx:6,ry:6
```

| Color | Significado |
|-------|-------------|
| 🟢 Verde | Objeto principal (Unidad Orgánica) |
| 🔵 Azul claro | Campos de datos (orgUnit) |
| 🟣 Violeta | Enums (roles) |

---

## 🔗 Código fuente

| Clase | Repositorio |
|-------|-------------|
| DN00OrgUnitReference | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/org/DN00OrgUnitReference.java) |
| DN00OrgUnitReferenceWithRole | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/org/DN00OrgUnitReferenceWithRole.java) |
| DN00OrgUnitRole | [Ver código](https://github.com/DENA-Euskadi/dena-common-data-api/blob/develop/denaCommonDataAPIModelClasses/src/main/java/dena/api/data/model/org/DN00OrgUnitRole.java) |

---

## 🧪 Tests y ejemplos

> Repositorio: [DENA-Euskadi/dena-interop-common-data-test](https://github.com/DENA-Euskadi/dena-interop-common-data-test)

| Mock Factory | Repositorio |
|--------------|-------------|
| DN99DENATestMockObjFactoryForOrgUnitReference | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/DN99DENATestMockObjFactoryForOrgUnitReference.java) |
| DN99DENATestMockObjFactoryForOrgUnitReferenceWithRole | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/DN99DENATestMockObjFactoryForOrgUnitReferenceWithRole.java) |
| DN99DENATestMockObjFactoryForOrgUnitRole | [Ver código](https://github.com/DENA-Euskadi/dena-interop-common-data-test/blob/develop/denaTestCommonDataClasses/src/main/java/dena/test/common/data/DN99DENATestMockObjFactoryForOrgUnitRole.java) |

---

## Atributos JSON


| Campo | Tipo | Obligatorio | Ejemplo | Descripción |
|-------|------|:-----------:|---------|-------------|
| `orgUnit.oid` | `String` | ❌ | `"ORG-OID-001"` | Identificador técnico |
| `orgUnit.id` | `String` | ✅ | `"ORG-URBANISMO"` | Identificador de negocio |
| `orgUnit.dir3Id` | `String` | ❌ | `"EA0000001"` | Código DIR3 |
| `orgUnit.displayNameByLanguage` | `LanguageTexts` | ✅ | `{"SPANISH":"Dpto. Urbanismo"}` | Nombre de la unidad |
| `orgUnit.urls` | `Array` | ❌ | | URLs de la unidad |
| `role` | `String` | ✅ | `"RESPONSIBLE"` | Rol en el contexto |

---

## Roles (`role`)

| Código | Descripción |
|--------|-------------|
| `RESPONSIBLE` | Unidad responsable |
| `MANAGING` | Unidad gestora (tramitadora) |
| `INFORMER` | Unidad informante |
| `SOLVER` | Unidad resolutora |
| `OTHER` | Otro rol |

---

## Ejemplo JSON (dentro de un servicio/procedimiento)

```json
{
  "participatingOrgUnits": [
    {
      "orgUnit": {
        "oid": "ORG-OID-001",
        "id": "ORG-URBANISMO",
        "dir3Id": "EA0000001",
        "displayNameByLanguage": {
          "SPANISH": "Departamento de Urbanismo",
          "BASQUE": "Hirigintza Saila"
        }
      },
      "role": "RESPONSIBLE"
    },
    {
      "orgUnit": {
        "oid": "ORG-OID-002",
        "id": "ORG-MEDIO-AMBIENTE",
        "displayNameByLanguage": {
          "SPANISH": "Departamento de Medio Ambiente",
          "BASQUE": "Ingurumen Saila"
        }
      },
      "role": "INFORMER"
    }
  ]
}
```

---

## Notas para la administración

- `orgUnit.id` y `orgUnit.displayNameByLanguage` son obligatorios.
- `orgUnit.dir3Id` es opcional pero recomendado si la unidad tiene código DIR3 asignado.
- `displayNameByLanguage` debe incluir al menos textos en `SPANISH` y `BASQUE`.
- Una misma unidad puede aparecer con diferentes roles en distintos servicios/procedimientos.




---

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
