# DENA Stylesheets - Modular Structure

This directory contains CSS styles organized in a modular way to facilitate maintenance and code comprehension.

## Files and responsibilities

### 01-variables.css
- **DENA corporate colour palette**
- **Official typefaces (Manrope)**
- **Reusable CSS variables**
- **Google Fonts import**

### 02-material-overrides.css
- **Material for MkDocs overrides**
- **Base theme configuration**
- **Main colours and typography**
- **Links and base elements**

### 03-header-navigation.css
- **Main header with DENA logo**
- **Tab navigation**
- **Header interactive elements**
- **Hover effects and active states**

### 04-ui-elements.css
- **Sidebar navigation**
- **Buttons and form elements**
- **Search**
- **General UI elements**

### 05-content-elements.css
- **Admonitions (notices, warnings, etc.)**
- **Status badges and HTTP methods**
- **Grid cards with hover effects**
- **Content-specific elements**

### 06-mermaid-diagrams.css
- **Styles for Mermaid diagrams**
- **DENA typography in diagrams**
- **Corporate colours for graphic elements**
- **Responsive design for diagrams**

## Loading order

Files are loaded in numerical order to ensure the correct style cascade:

1. Variables and base configuration
2. Material Design overrides
3. Header and navigation
4. UI elements
5. Content elements
6. Specific diagrams

## Maintenance

- **Centralised variables**: Colour changes only in `01-variables.css`
- **Separation of responsibilities**: Each file has a specific purpose
- **Ease of debugging**: Errors easier to locate by module
- **Scalability**: Easy to add new modules without affecting existing ones

## Migration from extra.css

The original `extra.css` file has been split while maintaining all existing functionality but with better organisation and readability.
