# Sistema de Versionado DENA

Este documento explica cómo usar el sistema de versionado centralizado para repositorios y artefactos DENA.

## Uso de Variables en Documentación

### URLs Versionadas de Repositorios

```markdown
<!-- Ejemplo: Enlace al repositorio de documentación con versión específica -->
[Documentación DENA]({{ repos.docs_tree_public }})

<!-- Ejemplo: Enlace al conector base con versión específica -->
Ver código fuente: [dena-connector-base-rest](https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/e80a021g-dena-interop-connectors/e80a021g-dena-connector-base-rest/tree/v0.3.30-SNAPSHOT)
```

### Artefactos Maven Versionados

!!! warning "Gestión de versiones de dependencias"
    
    Al especificar dependencias Maven de componentes DENA, siempre verifique que la versión seleccionada sea compatible con su entorno de despliegue. Consulte la matriz de compatibilidad antes de actualizar versiones en producción.

```xml
<!-- Dependencia con versión específica -->
<dependency>
    <groupId>dena.api.common</groupId>
    <artifactId>denaCommonAPIModelClasses</artifactId>
    <version>v0.3.30-SNAPSHOT</version>
</dependency>
```

### Comandos con Versiones

```bash
# Compilar con versión específica
mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc

# Instalar snapshot con versión específica  
mvn clean install -s ~/.m2/settings-dena.xml -Pbatsdlc -Drevision=0.3.26

# Build Docker con versión específica
docker build -t dena-service:0.3.26 .
```

## Actualización de Versiones

Para actualizar todas las versiones del sistema, solo necesitas modificar el archivo `mkdocs-vars.yaml`:

### 1. Actualizar Tags de Repositorios

```yaml
tags:
  dena_common_api: "v0.4.0"  # Nueva versión
  dena_connector_base_rest: "v0.4.0"  # Nueva versión
  # ... resto de tags
```

### 2. Actualizar Versión Principal

```yaml
dena:
  version: "0.4.0"  # Nueva versión principal
  date: "2026-08-24"  # Nueva fecha de release
```

### 3. Regenerar Documentación

```bash
mkdocs build
```

## Ventajas del Sistema

1. **Centralización**: Un solo lugar para actualizar todas las versiones
2. **Consistencia**: Todas las URLs y referencias apuntan a la versión correcta
3. **Automatización**: Fácil actualización masiva de versiones
4. **Trazabilidad**: Historial claro de qué versiones se documentaron juntas
5. **Mantenibilidad**: Cambios rápidos para nuevas releases

## Ejemplos Prácticos

### Documentar Nueva Feature

```markdown
## Nuevo Endpoint Data-Retrieve v0.3.30-SNAPSHOT

La nueva funcionalidad está disponible en:
- API: [dena-common-api](https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/public/dena-common-api/tree/v0.3.30-SNAPSHOT)
- Ejemplos: [Postman Collections]({{ repos.docs_tree }}/docs/adjuntos/postman)

### Dependencia Maven

\```xml
<dependency>
    <groupId>dena.api.common</groupId>
    <artifactId>denaCommonAPIModelClasses</artifactId>
    <version>v0.3.30-SNAPSHOT</version>
</dependency>
\```
```

### Guía de Instalación

!!! warning "Verificación de compatibilidad de versiones"
    
    Antes de proceder con la instalación de cualquier componente DENA, asegúrese de que la versión seleccionada sea compatible con la infraestructura de destino y las versiones de otros componentes ya desplegados.

```markdown
## Instalar Conector Demo v0.3.30-SNAPSHOT

1. Clonar repositorio:
   \```bash
   git clone https://gitlab-q.batsdlc.itbatera.euskadi.eus/itbatera/convergencia/dena/e80a021-dena-interop/e80a021h-dena-interop-mocks/e80a021h-dena-connector-demo1-rest.git
   cd dena-connector-demo1-rest
   git checkout v0.3.30-SNAPSHOT
   \```

2. Compilar:
   \```bash
   mvn clean compile -s ~/.m2/settings-dena.xml -Pbatsdlc
   \```
```

## Mantenimiento

### Checklist para Nueva Release

- [ ] Actualizar `tags.*` en `mkdocs-vars.yaml`
- [ ] Actualizar `dena.version` y `dena.date`
- [ ] Verificar que todos los repositorios tienen el tag correspondiente
- [ ] Regenerar documentación con `mkdocs build`
- [ ] Validar enlaces en la documentación generada
- [ ] Commit y tag del repositorio de documentación

### Scripts de Automatización

```bash
#!/bin/bash
# update-versions.sh - Script para actualizar versiones

NEW_VERSION="v0.4.0"
NEW_DATE=$(date +%Y-%m-%d)

# Actualizar mkdocs-vars.yaml
sed -i "s/version: \".*\"/version: \"$NEW_VERSION\"/" mkdocs-vars.yaml
sed -i "s/date: \".*\"/date: \"$NEW_DATE\"/" mkdocs-vars.yaml

# Regenerar documentación
mkdocs build

echo "Versiones actualizadas a $NEW_VERSION"
```