# DENA Favicon

To create a favicon from the DENA logo, you need to convert the PNG image to ICO.

You can use online tools such as:
- https://favicon.io/favicon-converter/
- https://convertio.co/png-ico/

Or use ImageMagick:
```bash
convert docs/adjuntos/logos/Dena_RGB_Calado_H.png -resize 32x32 docs/adjuntos/logos/favicon.ico
```

Then add in mkdocs.yml:
```yaml
theme:
  favicon: adjuntos/logos/favicon.ico
```

## Current status

Currently the PNG is used as favicon directly in mkdocs.yml:
```yaml
favicon: adjuntos/logos/Dena_RGB_Calado_H.png
```

This works correctly in modern browsers that support PNG as favicon.
