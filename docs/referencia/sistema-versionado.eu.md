# DENA Bertsioketa-sistema

Dokumentu honek DENA biltegietarako eta artefaktuetarako bertsioketa-sistema zentralizatua nola erabili azaltzen du.

## Aldagaiak Dokumentazioan Erabiltzea

### Biltegien URL Bertsiokatuak

```markdown
<!-- Adibidea: Dokumentazio-biltegirako esteka bertsio espezifikoarekin -->
[DENA Dokumentazioa]({{ repos.docs_tree_public }})

<!-- Adibidea: Oinarrizko konektorearen esteka bertsio espezifikoarekin -->
Ikusi iturburu-kodea: [dena-connector-base-rest](https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/e80a021g-dena-interop-connectors/e80a021g-dena-connector-base-rest/tree/v0.3.30-SNAPSHOT)
```

### Maven Artefaktu Bertsiokatuak

!!! warning "Mendekotasunen bertsio-kudeaketa"
    
    DENA osagaien Maven mendekotasunak zehaztean, beti egiaztatu hautatutako bertsioa zure hedapen-ingurunearekin bateragarria dela. Kontsultatu bateragarritasun-matrizea produkzioan bertsioak eguneratu aurretik.

```xml
<!-- Mendekotasuna bertsio espezifikoarekin -->
<dependency>
    <groupId>dena.api.common</groupId>
    <artifactId>denaCommonAPIModelClasses</artifactId>
    <version>v0.3.30-SNAPSHOT</version>
</dependency>
```

### Komandoak Bertsioekin

```bash
# Konpilatu bertsio espezifikoarekin
mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc

# Instalatu snapshot bertsio espezifikoarekin  
mvn clean install -s ~/.m2/settings-dena.xml -Pbatsdlc -Drevision=0.3.26

# Docker build bertsio espezifikoarekin
docker build -t dena-service:0.3.26 .
```

## Bertsioak Eguneratzea

Sistemako bertsio guztiak eguneratzeko, `mkdocs-vars.yaml` fitxategia aldatu besterik ez duzu:

### 1. Biltegien Tagak Eguneratu

```yaml
tags:
  dena_common_api: "v0.4.0"  # Bertsio berria
  dena_connector_base_rest: "v0.4.0"  # Bertsio berria
  # ... gainerako tagak
```

### 2. Bertsio Nagusia Eguneratu

```yaml
dena:
  version: "0.4.0"  # Bertsio nagusi berria
  date: "2024-12-20"  # Release data berria
```

### 3. Dokumentazioa Birsortu

```bash
mkdocs build
```

## Sistemaren Abantailak

1. **Zentralizazioa**: Leku bakarra bertsio guztiak eguneratzeko
2. **Koherentzia**: URL eta erreferentzia guztiak bertsio zuzenera begira
3. **Automatizazioa**: Bertsio-eguneraketa masiboak erraz egiteko
4. **Trazabilitatea**: Zein bertsio dokumentatu ziren batera historia argia
5. **Mantentze-erraztasuna**: Aldaketa azkarrak release berrietarako

## Adibide Praktikoak

### Feature Berri Bat Dokumentatzea

```markdown
## Data-Retrieve Endpoint Berria v0.3.30-SNAPSHOT

Funtzionalitate berria hemen dago eskuragarri:
- API: [dena-common-api](https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/public/dena-common-api/tree/v0.3.30-SNAPSHOT)
- Adibideak: [Postman Collections]({{ repos.docs_tree }}/docs/adjuntos/postman)

### Maven Mendekotasuna

\```xml
<dependency>
    <groupId>dena.api.common</groupId>
    <artifactId>denaCommonAPIModelClasses</artifactId>
    <version>v0.3.30-SNAPSHOT</version>
</dependency>
\```
```

### Instalazio-gida

!!! warning "Bertsio-bateragarritasunaren egiaztapena"
    
    Edozein DENA osagai instalatzen hasi aurretik, ziurtatu hautatutako bertsioa helburu-azpiegiturrarekin eta dagoeneko hedatutako beste osagaien bertsioekin bateragarria dela.

```markdown
## Demo Konektorea Instalatu v0.3.30-SNAPSHOT

1. Biltegia klonatu:
   \```bash
   git clone https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/e80a021h-dena-interop-mocks/e80a021h-dena-connector-demo1-rest.git
   cd dena-connector-demo1-rest
   git checkout v0.3.30-SNAPSHOT
   \```

2. Konpilatu:
   \```bash
   mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc
   \```
```

## Mantentze-lanak

### Release Berri Baterako Zerrenda

- [ ] Eguneratu `tags.*` `mkdocs-vars.yaml`-en
- [ ] Eguneratu `dena.version` eta `dena.date`
- [ ] Egiaztatu biltegi guztiek dagokien taga dutela
- [ ] Birsortu dokumentazioa `mkdocs build`-ekin
- [ ] Balioztatu estekak sortutako dokumentazioan
- [ ] Commit eta tag dokumentazio-biltegian

### Automatizazio-scriptak

```bash
#!/bin/bash
# update-versions.sh - Bertsioak eguneratzeko scripta

NEW_VERSION="v0.4.0"
NEW_DATE=$(date +%Y-%m-%d)

# Eguneratu mkdocs-vars.yaml
sed -i "s/version: \".*\"/version: \"$NEW_VERSION\"/" mkdocs-vars.yaml
sed -i "s/date: \".*\"/date: \"$NEW_DATE\"/" mkdocs-vars.yaml

# Birsortu dokumentazioa
mkdocs build

echo "Bertsioak $NEW_VERSION-ra eguneratuta"
```
