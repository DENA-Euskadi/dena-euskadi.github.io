# :material-web: HTTP Headers

Todas las llamadas HTTP en DENA incluyen un conjunto de cabeceras estándar y personalizadas que proporcionan contexto, seguridad y trazabilidad.

---

## Request HTTP Headers

| Header | Descripción | Ejemplo |
|---|---|---|
| `Authorization` | Token JWT para autenticación | `Authorization: Bearer {token}` |
| `If-Modified-Since` | Fecha de cacheado en formato [RFC 7231](https://datatracker.ietf.org/doc/html/rfc7231) | |
| `User-Agent` | Datos del cliente (navegador, app, librería) que origina la petición. Ver [UserAgent](./modelo/user-agent.md) | |
| `Content-Type` | Tipo de contenido del mensaje (habitualmente `application/json`) | `Content-Type: application/json` |
| `Content-Digest` | Digest SHA-256 calculado sobre todo el mensaje | `Content-Digest: sha-256=<digest-value>` |
| `X-DENA-Data-Digest` | Digest SHA-256 calculado sobre la parte de datos (body) del mensaje | `X-DENA-Data-Digest: sha-256=<digest-value>` |
| `X-DENA-This-TimeStamp` | Instante (EPOCH) en el que se inició la petición en el componente que la origina | `1670374400` |
| `X-DENA-Origin-TimeStamp` | Instante (EPOCH) en el que se inició el flujo en el componente inicial (ej: app móvil). Se conserva inalterado entre componentes | `1670374400` |
| `X-DENA-Message-Correlation-Id` | UID generado por el componente que inició el flujo. Se conserva inalterado entre componentes | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |

---

## Response HTTP Headers

| Header | Descripción | Ejemplo |
|---|---|---|
| `Content-Type` | Tipo de contenido de la respuesta | `Content-Type: application/json` |
| `Content-Digest` | Digest SHA-256 del cuerpo de la respuesta | `Content-Digest: sha-256=<digest-value>` |
| `X-DENA-Message-Correlation-Id` | UID de correlación (eco del request) | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Instante (EPOCH) en el que se generó la respuesta | `1670374500` |

---

## Digest de seguridad

Las cabeceras `Content-Digest` y `X-DENA-Data-Digest` sirven para garantizar la **integridad** del mensaje:

- `Content-Digest`: hash SHA-256 de **todo el mensaje** HTTP (headers + body)
- `X-DENA-Data-Digest`: hash SHA-256 solo del **body** (datos)

Esto permite al receptor verificar que el mensaje no ha sido alterado en tránsito.

!!! tip "Correlación y trazabilidad"
    El header `X-DENA-Message-Correlation-Id` permite asociar todas las llamadas derivadas de una petición original, facilitando la depuración en sistemas distribuidos.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
