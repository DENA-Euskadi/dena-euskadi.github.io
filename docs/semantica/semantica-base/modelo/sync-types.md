# :material-sync: Tipos de Datos del Flujo Sync

Esta página documenta las estructuras de datos utilizadas en el **flujo de sincronización de metadatos** (Metadata-Sync / SRMD) entre administraciones y DENA-CORE.

---

## DenaInteroperableDataTypeSync

Estructura utilizada por un origen de datos / administración para enviar **meta-data sync para un tipo de dato interoperable**, informando sobre cambios, altas o borrados en datos de personas registradas en DENA.

### Atributos

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `interoperableDataTypeId` | `ID` | :material-check: | Identificador del tipo de dato (ej: `PAYMENTS`, `RECORDS`) |
| `personDataSync` | `Array(DenaPersonDataSync)` | :material-check: | Array con los cambios detectados por persona |

### Ejemplo

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

Estructura que informa sobre los cambios en datos de **una persona** para un tipo de dato interoperable.

### Atributos

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `personId` | `ID` | :material-check: | Identificador administrativo de la persona (DNI/NIF/NIE/Pasaporte) |
| `objectOid` | `OID` | :material-close: | Identificador único del objeto en el módulo de personas de DENA |
| `lastUpdated` | `UTCDateTime` | :material-check: | Fecha/hora de la última actualización del dato |
| `change` | `Enum` | :material-check: | Tipo de cambio detectado |

### Valores de `change`

| Valor | Descripción |
|---|---|
| `CHANGED` | Un dato existente ha sido modificado |
| `NEW` | Se ha creado un dato nuevo |
| `DELETED` | Se ha eliminado un dato |

---

## DenaPersonMetaDataSyncItem

Estructura utilizada por la UI (DENA-APP) para solicitar meta-data sync **de una persona** sobre todos los tipos de datos interoperables de todos los orígenes de datos.

### Atributos

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `adminId` | `ID` | :material-check: | Identificador de la administración en DENA |
| `dataSourceId` | `ID` | :material-check: | Identificador del origen de datos (data source) en DENA |
| `dataTypeId` | `ID` | :material-check: | Identificador del tipo de datos (data type) en DENA |
| `lastUpdateTS` | `TimeStamp` | :material-check: | Fecha de última modificación de algún registro del tipo de dato en el origen |

### Ejemplo

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

!!! info "Uso en la UI"
    La UI utiliza `lastUpdateTS` para calcular si necesita solicitar un RETRIEVE de los datos al origen. Para hacer el retrieve, la UI necesita conocer la URL de retrieve en DENA-CORE para cada origen de datos (disponible en el registro de orígenes de datos).

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
