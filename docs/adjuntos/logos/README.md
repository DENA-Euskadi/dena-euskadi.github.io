# Favicon DENA

Para crear un favicon desde el logo DENA, necesitas convertir la imagen PNG a ICO.

Puedes usar herramientas online como:
- https://favicon.io/favicon-converter/
- https://convertio.co/png-ico/

O usar ImageMagick:
```bash
convert docs/adjuntos/logos/Dena_RGB_Calado_H.png -resize 32x32 docs/adjuntos/logos/favicon.ico
```

Luego agregar en mkdocs.yml:
```yaml
theme:
  favicon: adjuntos/logos/favicon.ico
```

## Estado actual

Actualmente se usa el PNG como favicon directamente en mkdocs.yml:
```yaml
favicon: adjuntos/logos/Dena_RGB_Calado_H.png
```

Esto funciona correctamente en navegadores modernos que soportan PNG como favicon.