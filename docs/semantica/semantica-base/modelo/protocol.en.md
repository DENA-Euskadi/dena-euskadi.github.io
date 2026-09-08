# :material-transit-connection-variant: DENAProtocol

## Description

Object containing **message exchange protocol information**. It is used to provide the data required for the internal functioning of communication between components.

Includes:

- URLs (callback, consent repository, verification, etc.)
- Timeouts
- Hashes / Signatures
- Tokens

!!! info "HATEOAS"
    The URL collection in `protocol` provides functionality similar to [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) (Hypermedia as the Engine of Application State).

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `urls` | [UrlCollection](../../../arquitectura/tipos-dato-base.md#urls) | :material-close: | Collection of URLs indexed by ID that the receiving entity must interpret to provide functionality |
| `timeOut` | `TimeLapse` | :material-close: | Time limit from message reception to complete its processing (`TimeLapse` format, e.g. `"30s"`, `"1m"`) |

Class: `DN00InteropProtocol` (`@MarshallType(as="protocol")`).

---

## Example

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
    "timeOut": "30s"
  }
}
```

---

## Use cases

| Case | Use of `protocol` |
|---|---|
| Asynchronous invocation | The callback URL where the server will return the response is included |
| Consent verification | The URL where the administration can verify a consent is included |
| Time control | A maximum timeout is set for processing |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
