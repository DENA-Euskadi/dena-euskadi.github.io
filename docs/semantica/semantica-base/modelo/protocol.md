# :material-transit-connection-variant: DENAProtocol

## Descripción

Objeto que contiene información de **protocolo de intercambio de mensajes**. Se utiliza para proporcionar datos necesarios para el funcionamiento interno de la comunicación entre componentes.

Incluye:

- URLs (de callback, del repositorio de consentimientos, de verificación, etc.)
- Timeouts
- Hashes / Firmas
- Tokens

!!! info "HATEOAS"
    La colección de URLs en `protocol` permite obtener funcionalidad similar a [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) (Hypermedia as the Engine of Application State).

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `urls` | [UrlCollection](../../../arquitectura/tipos-dato-base.md#urls) | :material-close: | Colección de URLs indexadas por ID que la entidad receptora debe interpretar para proporcionar funcionalidad |
| `timeOutMillis` | `Integer` | :material-close: | Límite en milisegundos desde la recepción del mensaje para completar su procesado |

---

## Ejemplo

```json
{
  "protocol": {
    "urls": [
      {
        "id": "callback",
        "url": "https://interop.api.dena.eus/callback/async-result"
      },
      {
        "id": "consentVerify",
        "url": "https://interop.api.dena.eus/consent/verify"
      }
    ],
    "timeOutMillis": 30000
  }
}
```

---

## Casos de uso

| Caso | Uso de `protocol` |
|---|---|
| Invocación asíncrona | Se incluye la URL de callback donde el servidor devolverá la respuesta |
| Verificación de consentimientos | Se incluye la URL donde la administración puede verificar un consentimiento |
| Control de tiempos | Se establece un timeout máximo para el procesamiento |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
