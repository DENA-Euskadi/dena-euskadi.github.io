# :material-shield-check: DENAConsent

## Description

Object containing the reference to the **legal basis** (consent or normative authorization) that backs an interoperability request.

!!! warning "Data-Retrieve only"
    The `consent` block is only present in **data retrieval** (Data-Retrieve) messages. It allows the administration to verify that a legal basis exists enabling the exchange of the person's data.

---

## JSON Attributes

| Field | Type | Mandatory | Description |
|---|---|:---:|---|
| `consentOid` | `OID` | :material-check: | Unique consent identifier in the common repository |
| `consentURL` | `URL` | :material-check: | URL where the receiving party can find and download the consent details |
| `consentData` | `Object` | :material-close: | Some consent details (when granted, via which medium, until when, etc.) |

---

## Example

```json
{
  "consent": {
    "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentURL": "https://interop.api.dena.eus/consent/db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentData": {
      "grantedAt": "2025-06-15T10:30:00.000Z",
      "expiresAt": "2026-06-15T10:30:00.000Z",
      "grantedVia": "DENA_APP_ENROLLMENT"
    }
  }
}
```

---

## Verification by the administration

The administration can, at any time, access `consentURL` to:

1. Download all consent details
2. Obtain a **signed receipt** issued by the common repository
3. Verify the consent is still valid

!!! tip "Optional verification"
    DENA-CORE already verifies the existence of the legal basis before sending the request. The administration can trust this mechanism or additionally verify if deemed necessary.

---

## Relationship with the consent lifecycle

For more detail on how consents are managed in DENA, see: [:octicons-arrow-right-24: Consents](../consentimientos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
