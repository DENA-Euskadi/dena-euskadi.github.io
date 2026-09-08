# :material-speedometer: API Limits and Restrictions

Known operational parameters of the DENA API.

---

## Timeouts

| Operation | Timeout | Reference |
|-----------|---------|-----------|
| **Data-Retrieve** (DENA → your admin) | 30 seconds | Field `protocol.timeOut: "30s"` in the request |

!!! tip "Data-Retrieve: 30 seconds"
    Your endpoint must respond within 30 seconds. If you need more time for complex queries, contact the DENA team to configure an extended timeout for your connector.

---

## OAuth2 Token

| Parameter | Value | Reference |
|-----------|-------|-----------|
| **Token duration** | 300 seconds (5 min) | Field `expires_in` in the get-token endpoint response |
| **Recommended leeway** | ~60 seconds before expiration | Recommendation documented in Your System Calls DENA section and FAQ |
| **Grant type** | `client_credentials` | Only supported grant type |

---

## Values pending documentation

!!! warning "Pending definition"
    The following operational parameters are not currently documented. Contact the DENA team if you need this information for your integration:
    
    - **Rate limits**: maximum number of requests per minute/hour per administration
    - **Maximum payload size**: body size limit in requests and responses
    - **Retry policy**: DENA behaviour when your endpoint returns errors
    - **Availability SLA**: guaranteed uptime percentage per environment (PRE/PRO)
    - **API versioning**: compatibility policy and communication of breaking changes
    - **Maintenance windows**: planned unavailability schedules

---

## General recommendations

Based on best practices and the documented system behaviour:

- **Cache the token** while it is valid. Do not request a new token on every call.
- **Renew the token** in advance (~60s before expiry) to avoid rejections due to network latency.
- **Respond quickly**: the 30s timeout is the maximum, but the faster you respond, the better the experience for the person.
- **Always return HTTP 200** with `dataItems: []` when there is no data. Do not use 404.

---

!!! question "Need information about limits?"
    
    For enquiries about rate limits, maximum payload sizes or SLA:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
