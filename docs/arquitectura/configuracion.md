# :material-cog: Configuración de DENA

DENA es configurable en estas áreas:

- **Sistema de etiquetado** que permite etiquetar activos con [labels] normalizados
- **Estructura organizacional** de las [Admins] que participan en el sistema
- **Interop**: los [tipos de dato] soportados por DENA y los [orígenes de datos] que proporcionan datos para esos [tipos de dato]

---

## Sistema de etiquetado (Labeling)

El sistema de [etiquetado] contiene [**labels**] que pueden usarse para categorizar cualquier activo.

!!! example "Ejemplo"
    Los [tipos de dato] se categorizan temáticamente: un [certificado de nacimiento] puede categorizarse con la [label] `familia`.

### Taxonomías

El sistema puede contener cientos de [labels] de diferentes [categorías] así que se necesita un medio para "seleccionar" las [labels]. Ese medio es la [**taxonomía**], que no es más que una disposición jerárquica de [labels]:

```
+ social
    + familia
    + jóvenes
    + personas mayores
    + inmigración
    + ...
+ cultura
    + teatro
    + lectura
    + ...
```

La [**taxonomía**] es **solo una herramienta para seleccionar una [label]** que es lo que realmente importa: la [label] es lo que se "pega" al activo categorizado.

### Objetos principales del sistema de etiquetado

```
+ [domain group]
    + [domain]
        + [label]
        + [taxonomy group]
            + [taxonomy]
```

#### Label

Una **[label]** representa un concepto o una categoría:

- Un **identificador único** (oid)
- **[Términos] estándar** en diferentes idiomas (solo **uno** por idioma)
- **[Términos] sinónimos** en diferentes idiomas (pueden existir **varios** por idioma)
- **Identificadores externos**: el concepto puede existir en otros sistemas con otros identificadores

#### Taxonomy

Una [**taxonomía**] es una herramienta para seleccionar [labels]:

- Identificador único (oid)
- Nombre en diferentes idiomas
- Estructura jerárquica de [labels] (los [labels] pueden repetirse en una taxonomía)

Internamente una **[taxonomía]** se representa como un *[node outline]* donde cada **[node]** contiene:

![Taxonomy Node Outline](../adjuntos/imagenes/arquitectura/taxonomy-node-outline.png)

- Un **identificador único**
- El **[node] padre** (null si es raíz)
- **La [label] a la que enlaza el [node]** y el [domain] que la contiene
- El **[role]** de la [label] en la [taxonomía] (normalmente la profundidad, pero puede ser cualquier string como "theme" o "subtheme")

### Java API para Labeling

Interfaz principal: `DN00ClientAPIForLabeling`

=== "Crear Domain Group"
    ```java
    DN00LabelingDomainGroup domainGroup = DN00LabelingDomainGroupBuilder.create()
        .suppliedWithNewOid()
        .defaultGroup()
        .noDescription()
        .withNames(name)
        .build();
    DN00LabelingDomainGroup saved = api.domainGroupAPI()
        .getForCRUD()
        .create(domainGroup);
    ```

=== "Crear Domain"
    ```java
    DN00LabelingDomain domain = DN00LabelingDomainBuilder.create()
        .suppliedWithNewOid()
        .noSecurityUID()
        .inGroup(domainGroupOid)
        .noDescription()
        .withNames(name)
        .build();
    DN00LabelingDomain saved = api.domainAPI()
        .getForCRUD()
        .create(domain);
    ```

=== "Crear Label"
    ```java
    DN00Label label = DN00LabelBuilder.create()
        .withOid(DN00LabelOID.supply())
        .inDomain(domainOid)
        .enabled()
        .withStandardTerms(stdTerms)
        .noSynonimousTerms()
        .build();
    DN00Label saved = api.labelAPI()
        .getForCRUD()
        .create(label);
    ```

=== "Crear Taxonomy"
    ```java
    DN00Taxonomy taxonomy = DN00TaxonomyBuilder.create()
        .withOid(DN00TaxonomyOID.supply())
        .withBusinessIds(DN00TaxonomyID.forId("taxonomy"))
        .inDomain(domainOid)
        .inGroup(groupOid)
        .toBeUsedFor(DN00TaxonomyUsage.ALL)
        .withNames(desc)
        .build();
    DN00Taxonomy saved = api.taxonomyAPI()
        .getForCRUD()
        .save(taxonomy);
    ```

=== "Crear Nodes"
    ```java
    // Root node
    DN00TaxonomyNode rootNode = api.taxonomyNodesAPI()
        .getForCRUD()
        .createRootNode(taxonomyOid,
                        labelOid, domainContainingLabelOid,
                        role);

    // Child node
    DN00TaxonomyNode childNode = api.taxonomyNodesAPI()
        .getForCRUD()
        .createNode(taxonomyOid,
                    labelOid, domainContainingLabelOid,
                    parentNodeOid,
                    role);
    ```

=== "Cargar vista de taxonomía"
    ```java
    DN00TaxonomyView taxonomyView = api.taxonomyNodesAPI()
        .getForNodeLabels()
        .usingCache()
        .loadTaxonomyView(taxonomy.getOid());
    ```

---

## Org Config

La [org config] contiene información sobre las [admins] que participan en DENA ofreciendo [datos].

Se organiza en dos niveles:

```
+ [org groups]
    + [admins]
```

!!! example "Ejemplo de estructura organizacional"
    ```
    + Administración General de la CAV
        + Gobierno Vasco
        + Osakidetza
        + Lanbide
    + Administración Foral de Bizkaia
        + Diputación Foral de Bizkaia
    + Administración Local de Bizkaia
        + Ayuntamiento de Bilbao
        + Ayuntamiento de Barakaldo
        + Ayuntamiento de Getxo
    + Administración Foral de Gipuzkoa
        + Diputación Foral de Gipuzkoa
    + Administración Local de Gipuzkoa
        + Ayuntamiento de Donostia-San Sebastián
        + Ayuntamiento de Irún
    + Administración Foral de Araba
        + Diputación Foral de Araba
    + Administración Local de Araba
        + Ayuntamiento de Vitoria-Gasteiz
    ```

Un **[org group]** se compone de: identificador único (oid/uid) + nombre en múltiples idiomas.

Un **[admin]** se compone de:

- Identificador único (oid/uid)
- El [org group] al que pertenece
- Nombre en múltiples idiomas
- Sync and Retrieve config:
    - **Prioridad para [data retrieval]** en DENA-APP
    - **Cold Start [data types]**: tipos de dato forzados al inscribirse una [persona]

---

## Interop Config

### Data Type

Un [data type] es un objeto de config simple:

- El **nombre** en diferentes idiomas
- Las [**labels**] en que se categoriza el [tipo de dato] (usadas por la app para "pintar" la UI por temas)

### Data Origin

El [data origin] config es la **tabla de routing** de DENA para los [data providers] de las [admins], usando:

- La [admin]
- El [tipo de dato]
- El [data origin instance]

Estructura:

```
+ [data origin]
    + (N) [dataType] config ........... cada [data type] tiene su propia [connector config]
        + [connector config] .......... cómo CONECTAR para RECUPERAR datos
            + (1) EXTERNAL side config  cómo el [connector] llama a la [admin]
            + (N) [admin] ............. admins a las que aplica esta config
    + (1) INTERNAL side config ........ cómo DENA-CORE llama al [connector]
```

#### Opciones arquitectónicas de los [data providers]

![Data Origin Architectures](../adjuntos/imagenes/arquitectura/data-origin-architectures.png)

| Opción | Descripción |
|--------|-------------|
| **(a)** Single | Un [data provider] para un **único [data type] en una [admin]** |
| **(b)** Aggregated instances | Un [data provider] que **agrega** [datos] de múltiples instancias de un sistema (ej: múltiples gestores de expedientes en un data lake) |
| **(c)** Distributed | **Múltiples [data providers]** para múltiples orígenes en diferentes instancias |
| **(d)** Aggregated admins | Un [data provider] que agrega [datos] de **múltiples [admins]** (ej: diputaciones forales que proporcionan datos para múltiples ayuntamientos) |

---

## AppVersion Config

DENA-CORE almacena datos históricos sobre las **versiones de la [app]** (móvil/web):

| Campo | Descripción |
|-------|-------------|
| **Version number** | Patrón: `{major}.{minor}.{patch}[-alias]` |
| **Developer preview** | Si es una preview de desarrollo |
| **Life range** | start..end (si end es null, es la versión actual) |
| **Status** | PERMANENT / TEMPORARY DISABLED / ENABLED |
| **Client platform** | MOBILE_ANDROID / MOBILE_IOS / WEB_BROWSER |
| **Messages** | Mensajes a mostrar tras una actualización |

Se usa principalmente en la **fase init** de la [client app] para:

- **Deshabilitar** la app si la versión instalada ya no es usable
- **Forzar** al cliente a **actualizar** la versión

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
