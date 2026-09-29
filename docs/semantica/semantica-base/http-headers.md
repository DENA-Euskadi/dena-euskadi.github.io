# :material-web: HTTP Headers

Todas las llamadas HTTP en DENA incluyen un conjunto de cabeceras estándar y personalizadas que proporcionan contexto, seguridad y trazabilidad.

---

## Request HTTP Headers

| Header | Descripción | Ejemplo |
|---|---|---|
| `Authorization` | Token JWT para autenticación (cabecera HTTP estándar; no la gestiona el traffic-flow de DENA) | `Authorization: Bearer {token}` |
| `User-Agent` | Datos del cliente (navegador, app, librería) que origina la petición. Ver [UserAgent](./modelo/user-agent.md) | |
| `Content-Type` | Tipo de contenido del mensaje (habitualmente `application/json`) | `Content-Type: application/json` |
| `Content-Digest` | Digest SHA-256 calculado **solo sobre el cuerpo (body)** del mensaje, codificado en Base64 | `Content-Digest: SHA-256=:<base64>:` |
| `X-DENA-Data-Digest` | Digest SHA-256 calculado sobre la concatenación `X-DENA-This-TimeStamp + X-DENA-Message-Correlation-Id + body`, codificado en Base64 | `X-DENA-Data-Digest: SHA-256=:<base64>:` |
| `X-DENA-This-TimeStamp` | Instante (EPOCH en milisegundos) en el que se inició la petición en el componente que la origina | `1670374400000` |
| `X-DENA-Origin-TimeStamp` | Instante (EPOCH en milisegundos) en el que se inició el flujo en el componente inicial (ej: app móvil). Se conserva inalterado entre componentes | `1670374400000` |
| `X-DENA-Message-Correlation-Id` | UID generado por el componente que inició el flujo. Se conserva inalterado entre componentes | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |

!!! warning "Cabeceras obligatorias del traffic-flow"

    En las llamadas de interoperabilidad protegidas por el *traffic-flow*, DENA **exige** las cabeceras `X-DENA-This-TimeStamp`, `X-DENA-Origin-TimeStamp`, `X-DENA-Message-Correlation-Id` y `X-DENA-Data-Digest`. Si falta alguna, la petición se rechaza con **HTTP 400**. Si `X-DENA-Data-Digest` o `Content-Digest` faltan o no cuadran con el body recibido, se rechaza con **HTTP 401**.

---

## Response HTTP Headers

| Header | Descripción | Ejemplo |
|---|---|---|
| `Content-Type` | Tipo de contenido de la respuesta | `Content-Type: application/json` |
| `Content-Digest` | Digest SHA-256 del cuerpo de la respuesta (Base64) | `Content-Digest: SHA-256=:<base64>:` |
| `X-DENA-Message-Correlation-Id` | UID de correlación (eco del request) | `db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb` |
| `X-DENA-This-TimeStamp` | Instante (EPOCH en milisegundos) en el que se generó la respuesta | `1670374500000` |

---

## Digest de seguridad

Las cabeceras `X-DENA-Data-Digest` y `Content-Digest` sirven para garantizar la **integridad** del mensaje. Ambas usan **SHA-256** y el hash resultante se codifica en **Base64**:

- `X-DENA-Data-Digest`: hash SHA-256 de la concatenación `X-DENA-This-TimeStamp + X-DENA-Message-Correlation-Id + body`. Liga el body a su timestamp y a su id de correlación.
- `Content-Digest`: hash SHA-256 **solo del body** (los datos). No incluye las cabeceras.

Formato del valor en ambas cabeceras: `SHA-256=:<hash-base64>:` (nombre de algoritmo, delimitador `=:`, hash en Base64 y `:` de cierre).

El receptor (filtro *traffic-flow* entrante) recalcula ambos digests sobre el body recibido y los compara; si no coinciden, rechaza la petición con **HTTP 401**.

Esto permite al receptor verificar que el mensaje no ha sido alterado en tránsito.

!!! tip "Correlación y trazabilidad"
    El header `X-DENA-Message-Correlation-Id` permite asociar todas las llamadas derivadas de una petición original, facilitando la depuración en sistemas distribuidos.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
