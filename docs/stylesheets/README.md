# DENA Stylesheets - Estructura Modular

Este directorio contiene los estilos CSS organizados de forma modular para facilitar el mantenimiento y la comprensión del código.

## Archivos y responsabilidades

### 01-variables.css
- **Paleta de colores corporativos DENA**
- **Tipografías oficiales (Manrope)**
- **Variables CSS reutilizables**
- **Importación de fuentes Google**

### 02-material-overrides.css
- **Overrides de Material for MkDocs**
- **Configuración del tema base**
- **Colores y tipografía principal**
- **Enlaces y elementos base**

### 03-header-navigation.css
- **Header principal con logo DENA**
- **Navegación por pestañas**
- **Elementos interactivos del header**
- **Efectos hover y estados activos**

### 04-ui-elements.css
- **Navegación lateral**
- **Botones y elementos de formulario**
- **Buscador**
- **Elementos de UI generales**

### 05-content-elements.css
- **Admonitions (avisos, warnings, etc.)**
- **Badges de estado y HTTP methods**
- **Grid cards con efectos hover**
- **Elementos específicos de contenido**

### 06-mermaid-diagrams.css
- **Estilos para diagramas de Mermaid**
- **Tipografía DENA en diagramas**
- **Colores corporativos para elementos gráficos**
- **Responsive design para diagramas**

## Orden de carga

Los archivos se cargan en orden numérico para asegurar la correcta cascada de estilos:

1. Variables y configuración base
2. Material Design overrides
3. Header y navegación
4. Elementos de UI
5. Elementos de contenido
6. Diagramas específicos

## Mantenimiento

- **Variables centralizadas**: Cambios de color solo en `01-variables.css`
- **Separación de responsabilidades**: Cada archivo tiene un propósito específico
- **Facilidad de debug**: Errores más fáciles de localizar por módulo
- **Escalabilidad**: Fácil añadir nuevos módulos sin afectar existentes

## Migración desde extra.css

El archivo `extra.css` original se ha dividido manteniendo toda la funcionalidad existente pero con mejor organización y legibilidad.