# DENA Versioning System

This document explains how to use the centralised versioning system for DENA repositories and artefacts.

## Using Variables in Documentation

### Versioned Repository URLs

```markdown
<!-- Example: Link to the documentation repository with a specific version -->
[DENA Documentation]({{ repos.docs_tree_public }})

<!-- Example: Link to the base connector with a specific version -->
See source code: [dena-connector-base-rest](https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/e80a021g-dena-interop-connectors/e80a021g-dena-connector-base-rest/tree/v0.3.30-SNAPSHOT)
```

### Versioned Maven Artefacts

!!! warning "Dependency version management"
    
    When specifying Maven dependencies for DENA components, always verify that the selected version is compatible with your deployment environment. Consult the compatibility matrix before updating versions in production.

```xml
<!-- Dependency with specific version -->
<dependency>
    <groupId>dena.api.common</groupId>
    <artifactId>denaCommonAPIModelClasses</artifactId>
    <version>v0.3.30-SNAPSHOT</version>
</dependency>
```

### Commands with Versions

```bash
# Compile with specific version
mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc

# Install snapshot with specific version  
mvn clean install -s ~/.m2/settings-dena.xml -Pbatsdlc -Drevision=0.3.26

# Docker build with specific version
docker build -t dena-service:0.3.26 .
```

## Updating Versions

To update all versions in the system, you only need to modify the `mkdocs-vars.yaml` file:

### 1. Update Repository Tags

```yaml
tags:
  dena_common_api: "v0.4.0"  # New version
  dena_connector_base_rest: "v0.4.0"  # New version
  # ... remaining tags
```

### 2. Update Main Version

```yaml
dena:
  version: "0.4.0"  # New main version
  date: "2024-12-20"  # New release date
```

### 3. Regenerate Documentation

```bash
mkdocs build
```

## System Advantages

1. **Centralisation**: A single place to update all versions
2. **Consistency**: All URLs and references point to the correct version
3. **Automation**: Easy bulk version updates
4. **Traceability**: Clear history of which versions were documented together
5. **Maintainability**: Quick changes for new releases

## Practical Examples

### Documenting a New Feature

```markdown
## New Data-Retrieve Endpoint v0.3.30-SNAPSHOT

The new functionality is available at:
- API: [dena-common-api](https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/public/dena-common-api/tree/v0.3.30-SNAPSHOT)
- Examples: [Postman Collections]({{ repos.docs_tree }}/docs/adjuntos/postman)

### Maven Dependency

\```xml
<dependency>
    <groupId>dena.api.common</groupId>
    <artifactId>denaCommonAPIModelClasses</artifactId>
    <version>v0.3.30-SNAPSHOT</version>
</dependency>
\```
```

### Installation Guide

!!! warning "Version compatibility check"
    
    Before proceeding with the installation of any DENA component, ensure that the selected version is compatible with the target infrastructure and the versions of other already-deployed components.

```markdown
## Install Demo Connector v0.3.30-SNAPSHOT

1. Clone repository:
   \```bash
   git clone https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/e80a021h-dena-interop-mocks/e80a021h-dena-connector-demo1-rest.git
   cd dena-connector-demo1-rest
   git checkout v0.3.30-SNAPSHOT
   \```

2. Compile:
   \```bash
   mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc
   \```
```

## Maintenance

### Checklist for a New Release

- [ ] Update `tags.*` in `mkdocs-vars.yaml`
- [ ] Update `dena.version` and `dena.date`
- [ ] Verify that all repositories have the corresponding tag
- [ ] Regenerate documentation with `mkdocs build`
- [ ] Validate links in the generated documentation
- [ ] Commit and tag the documentation repository

### Automation Scripts

```bash
#!/bin/bash
# update-versions.sh - Script to update versions

NEW_VERSION="v0.4.0"
NEW_DATE=$(date +%Y-%m-%d)

# Update mkdocs-vars.yaml
sed -i "s/version: \".*\"/version: \"$NEW_VERSION\"/" mkdocs-vars.yaml
sed -i "s/date: \".*\"/date: \"$NEW_DATE\"/" mkdocs-vars.yaml

# Regenerate documentation
mkdocs build

echo "Versions updated to $NEW_VERSION"
```
