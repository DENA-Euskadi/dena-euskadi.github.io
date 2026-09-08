# :material-cog: DENAren konfigurazioa

DENA arlo hauetan da konfiguragarria:

- **Etiketatze-sistema** aktiboak [labels] normalizatuekin etiketatzeko aukera ematen duena
- **Egitura organizatiboa**, sisteman parte hartzen duten [Admins] direlakoena
- **Interop**: DENAk onartzen dituen [data types] motak eta [data type] mota horietarako datuak ematen dituzten [data origins] jatorriak

---

## Etiketatze-sistema (Labeling)

[Etiketatze]-sistemak edozein aktibo kategorizatzeko erabil daitezkeen [**labels**] ditu.

!!! example "Adibidea"
    [data types] motak gaika kategorizatzen dira: [jaiotza-agiri] bat `familia` [label] etiketarekin kategoriza daiteke.

### Taxonomiak

Sistemak [kategoria] ezberdinetako ehunka [labels] izan ditzake, beraz [labels] "hautatzeko" bitarteko bat behar da. Bitarteko hori [**taxonomia**] da, eta [labels] etiketen antolamendu hierarkiko bat besterik ez da:

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

[**Taxonomia**] **[label] bat hautatzeko tresna bat baino ez da**, eta hori da benetan garrantzitsua dena: kategorizatutako aktiboari "itsasten" zaiona [label] da.

### Etiketatze-sistemaren objektu nagusiak

```
+ [domain group]
    + [domain]
        + [label]
        + [taxonomy group]
            + [taxonomy]
```

#### Label

**[label]** batek kontzeptu bat edo kategoria bat adierazten du:

- **Identifikatzaile bakar** bat (oid)
- **[Termino] estandarrak** hizkuntza ezberdinetan (bakarra hizkuntza bakoitzeko)
- **[Termino] sinonimoak** hizkuntza ezberdinetan (**hainbat** egon daitezke hizkuntza bakoitzeko)
- **Kanpoko identifikatzaileak**: kontzeptua beste sistema batzuetan egon daiteke beste identifikatzaile batzuekin

#### Taxonomy

[**Taxonomia**] bat [labels] hautatzeko tresna bat da:

- Identifikatzaile bakarra (oid)
- Izena hizkuntza ezberdinetan
- [labels] etiketen egitura hierarkikoa (taxonomia batean [labels] etiketak errepika daitezke)

Barnean **[taxonomia]** bat *[node outline]* gisa adierazten da, non [node] bakoitzak hau baitu:

![Taxonomy Node Outline](../adjuntos/imagenes/arquitectura/taxonomy-node-outline.png)

- **Identifikatzaile bakar** bat
- **[node] gurasoa** (null bada, erroa da)
- **[node]-ak lotzen duen [label] etiketa** eta hura duen [domain] domeinua
- [label] etiketaren **[role]** rola [taxonomia]-n (normalean sakonera, baina edozein string izan daiteke, adibidez "theme" edo "subtheme")

### Labeling-erako Java API

Interfaze nagusia: `DN00ClientAPIForLabeling`

=== "Domain Group sortu"
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

=== "Domain sortu"
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

=== "Label sortu"
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

=== "Taxonomy sortu"
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

=== "Nodes sortu"
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

=== "Taxonomiaren bista kargatu"
    ```java
    DN00TaxonomyView taxonomyView = api.taxonomyNodesAPI()
        .getForNodeLabels()
        .usingCache()
        .loadTaxonomyView(taxonomy.getOid());
    ```

---

## Org Config

[org config]-ak DENAn [datuak] eskainiz parte hartzen duten [admins] administrazioei buruzko informazioa du.

Bi mailatan antolatzen da:

```
+ [org groups]
    + [admins]
```

!!! example "Egitura organizatiboaren adibidea"
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

**[org group]** bat honela osatzen da: identifikatzaile bakarra (oid/uid) + izena hizkuntza anitzetan.

**[admin]** bat honela osatzen da:

- Identifikatzaile bakarra (oid/uid)
- Zein [org group] taldetakoa den
- Izena hizkuntza anitzetan
- Sync and Retrieve config:
    - **[data retrieval]-erako lehentasuna** DENA-APPn
    - **Cold Start [data types]**: [persona] bat inskribatzean behartutako datu motak

---

## Interop Config

### Data Type

[data type] bat konfigurazio-objektu sinple bat da:

- **Izena** hizkuntza ezberdinetan
- [data type] mota zein [**labels**] etiketatan kategorizatzen den (aplikazioak UIa gaika "margotzeko" erabiltzen ditu)

### Data Origin

[data origin] config-a DENAren **routing taula** da, [admins] administrazioen [data providers] hornitzaileentzat, hauek erabiliz:

- [admin]
- [data type] mota
- [data origin instance]

Egitura:

```
+ [data origin]
    + (N) [dataType] config ........... [data type] bakoitzak bere [connector config] du
        + [connector config] .......... nola KONEKTATU datuak BERRESKURATZEKO
            + (1) EXTERNAL side config  nola deitzen dion [connector]-ak [admin]-ari
            + (N) [admin] ............. config hau zein admin-i aplikatzen zaien
    + (1) INTERNAL side config ........ nola deitzen dion DENA-COREk [connector]-ari
```

#### [data providers]-en aukera arkitektonikoak

![Data Origin Architectures](../adjuntos/imagenes/arquitectura/data-origin-architectures.png)

| Aukera | Deskribapena |
|--------|-------------|
| **(a)** Single | [data provider] bat **[admin] bakar batean [data type] mota bakarrerako** |
| **(b)** Aggregated instances | Sistema baten hainbat instantziatako [datuak] **agregatzen** dituen [data provider] bat (adib.: espediente-kudeatzaile anitz data lake batean) |
| **(c)** Distributed | **[data providers] anitz** instantzia ezberdinetako jatorri anitzetarako |
| **(d)** Aggregated admins | **[admins] anitzetako** [datuak] agregatzen dituen [data provider] bat (adib.: foru-aldundiak udal anitzetarako datuak ematen dituztenak) |

---

## AppVersion Config

DENA-COREk [app]-aren **bertsioei** (mugikorra/weba) buruzko datu historikoak gordetzen ditu:

| Eremua | Deskribapena |
|-------|-------------|
| **Version number** | Eredua: `{major}.{minor}.{patch}[-alias]` |
| **Developer preview** | Garapen-aurrebista bat den ala ez |
| **Life range** | start..end (end null bada, uneko bertsioa da) |
| **Status** | PERMANENT / TEMPORARY DISABLED / ENABLED |
| **Client platform** | MOBILE_ANDROID / MOBILE_IOS / WEB_BROWSER |
| **Messages** | Eguneratze baten ondoren erakusteko mezuak |

Batez ere [client app]-aren **init fasean** erabiltzen da honetarako:

- Aplikazioa **desgaitzeko**, instalatutako bertsioa jada erabilgarria ez bada
- Bezeroa bertsioa **eguneratzera behartzeko**

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
