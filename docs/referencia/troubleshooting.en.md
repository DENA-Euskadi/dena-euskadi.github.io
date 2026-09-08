# :material-lifebuoy: Troubleshooting

Centralised guide for common errors and their resolution, organised by category.

---

## Connectivity

??? failure "Connection refused"

    **Symptom:** Cannot establish a connection to the endpoint.

    **Possible causes:**

    - Service not started
    - Port blocked by firewall
    - Incorrect IP/hostname

    **Solution:**

    ```bash
    # Verify the service is running
    curl -v http://localhost:8082/api/hello

    # Verify network connectivity
    telnet api-batera.pre.dena.eus 443

    # Verify DNS resolution
    nslookup api-batera.pre.dena.eus
    ```

??? failure "Connection timeout"

    **Symptom:** The connection starts but does not complete within the expected time.

    **Possible causes:**

    - Firewall silently blocking
    - Corporate proxy not configured
    - Network rules not opened

    **Solution:**

    - Verify firewall rules with the infrastructure team
    - Configure proxy: `export https_proxy=http://proxy:3128`
    - Request traffic opening towards DENA IPs

??? failure "SSL/TLS handshake error"

    **Symptom:** `javax.net.ssl.SSLHandshakeException` or similar.

    **Possible causes:**

    - Server certificate not recognised by the Java truststore
    - Incompatible TLS version
    - Expired certificate

    **Solution:**

    ```bash
    # Download the certificate
    openssl s_client -connect api-batera.pre.dena.eus:443 < /dev/null 2>/dev/null | \
      openssl x509 > dena-ca.crt

    # Import into the truststore
    keytool -import -trustcacerts -alias dena-ca \
      -file dena-ca.crt \
      -keystore $JAVA_HOME/lib/security/cacerts \
      -storepass changeit
    ```

---

## Authentication

??? failure "401 Unauthorized"

    **Symptom:** `{"error": "invalid_token"}` or HTTP 401 response.

    **Possible causes:**

    - Expired token
    - Token from the wrong environment (PRE token used in PRO)
    - Malformed token in the header

    **Solution:**

    - Verify the token has not expired (`expires_in`)
    - Regenerate the token using the correct endpoint
    - Verify header format: `Authorization: Bearer <token>` (with a space after Bearer)

??? failure "invalid_client when obtaining token"

    **Symptom:** `{"error": "invalid_client", "error_description": "Invalid client or Invalid client credentials"}`

    **Possible causes:**

    - Incorrect `client_id`
    - Incorrect `client_secret`
    - Credentials from a different environment

    **Solution:**

    - Verify `client_id` and `client_secret` (no leading/trailing whitespace)
    - Confirm the credentials belong to the correct environment (PRE vs PRO)
    - Contact the DENA team if the credentials do not work

??? failure "403 Forbidden"

    **Symptom:** Valid token but insufficient permissions.

    **Possible causes:**

    - The client does not have the required scopes/roles
    - The resource requires additional permissions

    **Solution:**

    - Contact the DENA team to review the client's permissions

---

## Data-Retrieve

??? failure "400 Bad Request — mandatory fields"

    **Symptom:** `{"code": "CLIENT_ERR", "details": "Missing required fields..."}`

    **Possible causes:**

    - Missing `context.message.type`
    - Missing `context.subjectPerson.id`
    - Missing `context.dataType.id`
    - Missing `context.message.correlationId`

    **Solution:**

    Verify that the JSON includes all mandatory fields:

    ```json
    {
      "context": {
        "message": {
          "type": "PERSON_FETCH_DATA",
          "correlationId": "uuid-here"
        },
        "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DTYPE-OID-RECORDS" },
        "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
      },
      "payload": {}
    }
    ```

??? failure "Empty response (dataItems: [])"

    **Symptom:** HTTP 200 but no data.

    **Possible causes:**

    - The person has no data of the requested type
    - The `subjectPerson.id` does not exist in the administration's system
    - The `dataType.id` is not recognised

    **Solution:**

    - Verify that the `subjectPerson.id` exists in your system
    - Verify that the `dataType.id` matches the types your administration manages
    - This may be correct behaviour (person with no records, for example)

??? failure "Response timeout"

    **Symptom:** DENA receives a timeout when calling your endpoint.

    **Possible causes:**

    - Slow database query
    - Overloaded service
    - Timeout configured too low

    **Solution:**

    - Optimise database queries
    - The standard timeout is 30s. Always respond within that margin
    - If you need more time, contact the DENA team

---

## Traffic Flow (DENA headers)

??? failure "Missing required context fields"

    **Symptom:** `{"status": 400, "message": "Missing required context fields: context.message.type..."}`

    **Possible causes:**

    - The body does not contain the `context` field
    - Mandatory fields are missing inside `context`

    **Solution:**

    Mandatory fields in the body:

    - `context.message.type`
    - `context.message.correlationId`
    - `context.message.interopRouteData`
    - `context.originClientInstallment` or `context.originAdmin` (depending on the message origin)

??? failure "Hash mismatch (X-DENA-Data-Digest)"

    **Symptom:** Rejection due to incorrect hash in DENA headers.

    **Possible causes:**

    - The body was modified after calculating the hash
    - Proxy/middleware altering the body
    - Incorrect encoding (UTF-8 expected)

    **Solution:**

    - Calculate the hash on the exact body sent over the network
    - Do not modify the body after generating the digest
    - Verify there are no intermediate proxies altering the content

---

## Build and deployment

??? failure "BUILD FAILURE — unresolved dependencies"

    **Symptom:** Maven cannot find DENA/R01F artefacts.

    **Solution:**

    Verify `settings.xml`:

    ```xml
    <repository>
      <id>ejie-group</id>
      <url>https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-group/</url>
    </repository>
    ```

??? failure "java.lang.UnsupportedClassVersionError"

    **Symptom:** Error when running with an incorrect Java version.

    **Solution:**

    ```bash
    # Verify version
    java -version  # Must be 21+

    # Verify JAVA_HOME
    echo $JAVA_HOME
    ```

---

!!! tip "Can't find your error?"

    If the problem persists or you cannot find your specific error in this guide:
    
    **:material-email: Contact the DENA support team:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    Include in your query:
    
    - Full error message
    - Relevant logs
    - Environment (PRE/PRO/local)
    - `message.correlationId` if you have it
    - Context information (what you were trying to do)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
