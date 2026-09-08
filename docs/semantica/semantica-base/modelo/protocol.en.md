# :material-transit-connection-variant: DENAProtocol

## Description

Object containing **message exchange protocol information**. It provides data necessary for the internal functioning of communication between components.

Includes:

- URLs (callback, consent repository, verification, etc.)
- Timeouts
- Hashes / Signatures
- Tokens

!!! info "HATEOAS"
    The URL collection in `protocol` enables functionality similar to [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS) (Hypermedia as the Engine of Application State).

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `urls` | [UrlCollection](../../../arquitectura/tipos-dato-base.md#urls) | :material-close: | Collection of URLs indexed by ID that the receiving entity must interpret to provide functionality |
| `timeOutMillis` | `Integer` | :material-close: | Timeout in milliseconds from message reception to complete processing |

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
    "timeOutMillis": 30000
  }
}
```

---

## Use cases

| Case | Use of `protocol` |
|---|---|
| Asynchronous invocation | Includes the callback URL where the server will return the response |
| Consent verification | Includes the URL where the administration can verify a consent |
| Time control | Sets a maximum timeout for processing |

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
