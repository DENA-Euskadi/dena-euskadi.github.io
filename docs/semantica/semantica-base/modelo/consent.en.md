# :material-shield-check: Consent (consentOid)

## Description

The consent (or enabling legal basis) that backs a request is referenced by its **OID**. In the code model, the consent is transmitted as a single `consentOid` field (type `DN00ConsentOID`) in the base of requests (`DN00InteropRequestMessageBase`).

!!! info "Present in all requests"
    `consentOid` is part of the base of interoperability requests, not just Data-Retrieve. Its effective presence depends on whether the operation requires an enabling legal basis.

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `consentOid` | `OID` (`DN00ConsentOID`) | :material-close: | Unique consent identifier in the consent repository |

---

## Example

```json
{
  "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
}
```

---

## Verification

The current model only carries the consent OID. The consent detail (when it was granted, validity, etc.) is managed in DENA's consent repository; the query API for that repository is pending definition.

For more context on the consent lifecycle, see: [:octicons-arrow-right-24: Consents](../consentimientos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
