# :material-cogs: Operativas DENA

Esta sección contiene todas las operaciones funcionales que las administraciones pueden implementar para integrarse con la plataforma DENA.

## Operaciones Disponibles

<div class="grid cards" markdown>

-   :material-database: **Servir Datos**
    
    ---
    
    Implementa endpoints para que DENA pueda consultar datos de tu administración bajo demanda.
    
    [:octicons-arrow-right-24: Ir a Servir Datos](../semantica/data-retrieve/index.md)

-   :material-bell: **Notificar Cambios**
    
    ---
    
    Notifica a DENA cuando hay cambios en los metadatos de tu administración.
    
    [:octicons-arrow-right-24: Ir a Notificar Cambios](../semantica/metadata-sync/index.md)

-   :material-sync: **Sincronizar Personas**
    
    ---
    
    Mantén sincronizada la información de personas entre DENA y tu administración.
    
    [:octicons-arrow-right-24: Ir a Sincronizar Personas](../semantica/person-sync/index.md)

</div>

## Flujo Recomendado

1. **Comienza por Servir Datos** - Es la operativa fundamental que permite a DENA acceder a la información de tu administración
2. **Implementa Notificar Cambios** - Para mantener a DENA informado de actualizaciones en tiempo real  
3. **Configura Sincronizar Personas** - Para mantener coherencia en los datos personales

!!! tip "¿Por dónde empezar?"
    Si es tu primera integración con DENA, te recomendamos empezar por **Servir Datos** ya que es la operativa más básica y necesaria para cualquier integración.