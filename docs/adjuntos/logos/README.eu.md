# DENA Favicon-a

DENA logotipotik favicon bat sortzeko, PNG irudia ICO formatura bihurtu behar duzu.

Lineko tresnak erabil ditzakezu, hala nola:
- https://favicon.io/favicon-converter/
- https://convertio.co/png-ico/

Edo ImageMagick erabili:
```bash
convert docs/adjuntos/logos/Dena_RGB_Calado_H.png -resize 32x32 docs/adjuntos/logos/favicon.ico
```

Gero mkdocs.yml-en gehitu:
```yaml
theme:
  favicon: adjuntos/logos/favicon.ico
```

## Egungo egoera

Gaur egun PNG-a zuzenean favicon gisa erabiltzen da mkdocs.yml-en:
```yaml
favicon: adjuntos/logos/Dena_RGB_Calado_H.png
```

Honek ondo funtzionatzen du PNG favicon gisa onartzen duten nabigatzaile modernoetan.
