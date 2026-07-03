# 🌍 ALVARO — Guía de Traducción de Documentación DENA

## Contexto

Estamos traduciendo toda la documentación MkDocs de DENA (67 ficheros `.md`) del castellano al inglés (🇬🇧) y al euskera (🇪🇺). El sitio usa el plugin `mkdocs-static-i18n` con sufijos de idioma.

---

## Estructura de ficheros

Para cada fichero original en castellano:

```
docs/ejemplo/pagina.md        ← Original (castellano, idioma por defecto)
docs/ejemplo/pagina.en.md     ← Traducción al inglés
docs/ejemplo/pagina.eu.md     ← Traducción al euskera
```

Los ficheros traducidos van en la **misma carpeta** que el original, con el sufijo `.en.md` o `.eu.md` antes de la extensión.

---

## Qué se traduce

- ✅ Títulos y encabezados
- ✅ Texto descriptivo y párrafos
- ✅ Contenido de tablas (headers y celdas)
- ✅ Admonitions (tip, warning, info, etc.) — tanto el título como el contenido
- ✅ Tabs (`=== "nombre del tab"`)
- ✅ **Diagramas mermaid** — textos dentro de nodos, participantes, mensajes, notas y labels de flechas
- ✅ Comentarios en bloques de código bash (ej: `# 1. Clonar el repo`)
- ✅ Cards (grid cards de Material)
- ✅ Alt text de imágenes

## Qué NO se traduce

- ❌ URLs y enlaces (se mantienen igual)
- ❌ Nombres de ficheros y rutas
- ❌ Código fuente (Java, XML, JSON, YAML)
- ❌ Nombres técnicos (Data-Retrieve, Metadata-Sync, Person-Sync, OAuth2, etc.)
- ❌ Variables de mkdocs-macros (`{{ dena.version }}`, `{{ repos.docs_tree }}`, etc.)
- ❌ Iconos de Material (`:material-check:`, `:octicons-arrow-right-24:`, etc.)
- ❌ Estilos CSS de mermaid (themeVariables, style lines)
- ❌ El footer `<!-- DENA-DOC-FOOTER -->` y la línea `<sub>DENA Docs v{{ dena.version }}...</sub>`

---

## Proceso paso a paso

1. **Abrir el fichero original** (castellano) y leer su contenido
2. **Pegar el prompt en Amazon Q o Copilot** (ver sección de prompt más abajo) con el contenido del fichero
3. **Copiar la salida** del asistente y crear los dos ficheros:
   - `fichero.en.md` — traducción al inglés
   - `fichero.eu.md` — traducción al euskera
4. **Verificar en `mkdocs serve`** — acceder a la URL correspondiente:
   - 🇪🇸 `http://localhost:8000/<ruta>/`
   - 🇬🇧 `http://localhost:8000/en/<ruta>/`
   - 🇪🇺 `http://localhost:8000/eu/<ruta>/`
5. **Revisar visualmente** que:
   - Los diagramas mermaid renderizan correctamente
   - Las tablas se ven bien
   - Los admonitions y tabs funcionan
   - No hay variables `{{ }}` rotas
6. **Marcar como completado** en `TRANSLATION_TRACKER.md` (poner ✅ en columnas EN y EU)
7. **Pasar al siguiente** fichero de la lista

---

## Prompt para Amazon Q / Copilot

Copia y pega este prompt en el chat de Amazon Q o Copilot. Sustituye `<CONTENIDO>` por el contenido del fichero `.md` que toca traducir:

```
Traduce el siguiente fichero de documentación MkDocs al inglés y al euskera.

Reglas:
- Traduce TODO el texto: títulos, párrafos, tablas, admonitions, tabs, contenido de diagramas mermaid (nodos, participantes, mensajes, labels de flechas, notas), comentarios en bloques bash, alt text de imágenes y cards.
- NO traduzcas: URLs, rutas de ficheros, código fuente (Java/XML/JSON/YAML), nombres técnicos (Data-Retrieve, Metadata-Sync, Person-Sync, OAuth2, WebAuthn, Giltza), variables {{ }}, iconos :material-*: / :octicons-*:, estilos CSS de mermaid (themeVariables, style lines), ni el footer DENA-DOC-FOOTER.
- Mantén EXACTAMENTE la misma estructura markdown, indentación y formato.
- Los ficheros de salida deben ser completos y funcionales.
- Dame primero el .en.md completo y luego el .eu.md completo, separados claramente.

Fichero original (castellano):

<CONTENIDO>
```

### Ejemplo de uso con Amazon Q en IDE

1. Abre el chat de Amazon Q en tu IDE
2. Escribe `@` y selecciona el fichero `.md` que quieres traducir (así no tienes que copiar/pegar el contenido)
3. Pega el prompt de arriba (sin la parte de `<CONTENIDO>` ya que el fichero ya está referenciado)
4. Amazon Q te devolverá los dos ficheros traducidos
5. Crea los ficheros `.en.md` y `.eu.md` en la misma carpeta que el original

### Ejemplo de uso con Copilot

1. Abre el fichero `.md` original en el editor
2. Abre el chat de Copilot
3. Pega el prompt con el contenido del fichero
4. Copilot te devolverá las dos traducciones
5. Crea los ficheros `.en.md` y `.eu.md` manualmente

---

## Fichero de control

El progreso se lleva en `TRANSLATION_TRACKER.md` en la raíz del proyecto. Cada fichero tiene su estado:

| Símbolo | Estado |
|---|---|
| ⬜ | Pendiente |
| 🔄 | En revisión |
| ✅ | Completado |

Cuando termines un fichero, actualiza la fila correspondiente poniendo ✅ en las columnas EN y EU.

---

## Cómo ejecutar mkdocs en local

### Primera vez (instalar dependencias)

```bash
# 1. Asegúrate de tener Python 3.9+ instalado
python --version

# 2. (Recomendado) Crear un entorno virtual
python -m venv .venv
source .venv/bin/activate    # macOS/Linux
# .venv\Scripts\activate     # Windows

# 3. Instalar dependencias
pip install -r requirements.txt
```

### Dependencias (requirements.txt)

El fichero `requirements.txt` contiene:

```
mkdocs-material>=9.5,<10
mkdocs-macros-plugin>=1.0
mkdocs-static-i18n>=1.2,<2
```

- `mkdocs-material` — Tema Material para MkDocs
- `mkdocs-macros-plugin` — Variables reutilizables (`{{ dena.version }}`, etc.)
- `mkdocs-static-i18n` — Plugin de internacionalización (genera las versiones por idioma)

### Servir en local

```bash
# Desde la raíz del proyecto (donde está mkdocs.yml)
mkdocs serve
```

Esto levanta un servidor en `http://localhost:8000` con hot-reload (se actualiza automáticamente al guardar cambios).

### Acceder a cada idioma

| Idioma | URL base |
|---|---|
| 🇪🇸 Castellano (default) | `http://localhost:8000/` |
| 🇬🇧 English | `http://localhost:8000/en/` |
| 🇪🇺 Euskara | `http://localhost:8000/eu/` |

Para acceder a un fichero concreto, añade la ruta. Ejemplo para `guia-inicio/instalacion.md`:

| Idioma | URL |
|---|---|
| 🇪🇸 | `http://localhost:8000/guia-inicio/instalacion/` |
| 🇬🇧 | `http://localhost:8000/en/guia-inicio/instalacion/` |
| 🇪🇺 | `http://localhost:8000/eu/guia-inicio/instalacion/` |

### Build estático (opcional)

```bash
# Genera el sitio estático en la carpeta site/
mkdocs build
```

### Problemas comunes

| Problema | Solución |
|---|---|
| `ModuleNotFoundError: No module named 'material'` | Ejecuta `pip install -r requirements.txt` |
| `Plugin 'i18n' not found` | Ejecuta `pip install mkdocs-static-i18n` |
| Al cambiar de idioma me redirige a la home | Normal — el fichero aún no tiene su versión `.en.md` o `.eu.md` |
| Variables `{{ }}` aparecen como texto | Verifica que `mkdocs-macros-plugin` está instalado y que `mkdocs-vars.yaml` existe |

---

## Navegación entre idiomas

El selector de idioma (🇪🇸 🇬🇧 🇪🇺) aparece en la barra superior del tema Material.

⚠️ **Importante:** Si un fichero NO tiene su versión traducida, al navegar a él desde otro idioma el plugin hará fallback al castellano y te redirigirá a la home. Esto es normal hasta que todos los ficheros estén traducidos.

---

## Configuración relevante

| Fichero | Qué contiene |
|---|---|
| `mkdocs.yml` | Plugin i18n, nav, nav_translations para EN y EU |
| `mkdocs-vars.yaml` | Variables reutilizables (URLs, versiones) |
| `requirements.txt` | Dependencias Python (incluye `mkdocs-static-i18n`) |
| `TRANSLATION_TRACKER.md` | Lista de 67 ficheros con estado de traducción |

---

## Estado actual

- **Total:** 67 ficheros
- **Completados:** 16/67
- **Último traducido:** `docs/guia-inicio/instalacion.md` (#16)
- **Siguiente:** `docs/guia-inicio/mock-expedientes.md` (#17)

---

## Tips

- Si el fichero es un placeholder "En construcción", la traducción es muy corta — no te compliques.
- Los diagramas mermaid mantienen la misma estructura y estilos, solo cambian los textos de los nodos/mensajes.
- En euskera, los términos técnicos se dejan en su forma original (OAuth2, REST, endpoint, token, etc.).
- Revisa siempre que el mermaid renderiza bien en el navegador después de traducir.
- Si un fichero tiene imágenes PNG con texto en castellano, anótalo en la columna "Notas" del tracker (las imágenes no se traducen, solo el alt text).
