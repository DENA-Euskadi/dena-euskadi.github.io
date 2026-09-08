# :material-cellphone-link: UserAgent

## Descripción

El [User-Agent](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/User-Agent) es un `String` que forma parte de las peticiones HTTP como cabecera, proporcionando datos sobre el **cliente** (navegador, aplicación, etc.) que realiza la petición.

---

## Formato general

```
<product>/<version> (<system-information>) <platform> (<platform-details>) <extensions>
```

---

## Formato según origen del mensaje

### Mensaje iniciado por el cliente (mobile app / web app)

=== "App Móvil (Flutter)"

    ```
    DenaApp/<DenaApp ver> (<system> <System ver>; <device> <device ver>) Dart/<dart ver> Flutter/<flutter ver>
    ```

    **Ejemplo:**
    ```
    DenaApp/1.0 (Android 13; Pixel 6 Pro) Dart/3.0 Flutter/3.16
    ```

=== "App Web (navegador)"

    El User-Agent será el que determine el propio navegador web.

    **Ejemplo (Chrome en Mac):**
    ```
    Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/143.0.0.0 Safari/537.36
    ```

### Mensaje iniciado por DENA-CORE

```
DENA-CORE/<dena-core ver> <module>/<module ver> (support@dena.eus)
```

**Ejemplo** (módulo person-data notificando un alta):
```
DENA-CORE/2.1.0 person-data/1.0.1 (support@dena.eus)
```

### Mensaje iniciado por una administración

```
AdminX/<AdminX ver> <module>/<module ver> (support@adminX.eus)
```

**Ejemplo** (administración enviando SRMD de expedientes):
```
AdminX/1.1.0 expedientes/1.0.0 (support@adminX.eus)
```

!!! info "No obligatorio para administraciones"
    El header User-Agent en los mensajes iniciados por un origen de datos de una administración **no es obligatorio**.

---

## Componentes del User-Agent (ejemplo Chrome Android)

| Componente | Valor | Descripción |
|---|---|---|
| `<product>/<version>` | `Mozilla/5.0` | Compatibilidad histórica |
| `<system-information>` | `Linux; Android 16; Pixel 9` | SO y dispositivo |
| `<platform>` | `AppleWebKit/537.36` | Motor de renderizado |
| `<platform-details>` | `KHTML, like Gecko` | Detalles del motor |
| `<extensions>` | `Chrome/143.0.12.45 Mobile Safari/537.36` | Navegador y modo |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
