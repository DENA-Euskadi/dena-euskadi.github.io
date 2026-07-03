# :material-frequently-asked-questions: Frequently Asked Questions (FAQ)

Answers to the most common questions from integrating administrations.

---

## General integration

??? question "What do I need to start integrating with DENA?"

    1. **OAuth2 credentials** provided by the DENA team (`client_id` + `client_secret`)
    2. **HTTPS connectivity** to the DENA endpoints (PRE and/or PRO)
    3. **Java 21+** and **Maven 3.9+** to compile the sample/test projects

    [:octicons-arrow-right-24: Installation guide](../guia-inicio/instalacion.md)

??? question "Which endpoints do I have to implement mandatorily?"

    It depends on your use case:

    | Case | Mandatory endpoint |
    |---|---|
    | DENA queries data from your administration | `POST /api/retrieveData` ([Data-Retrieve](../semantica/data-retrieve/index.md)) |
    | You notify changes to DENA | `POST` to the DENA [Metadata-Sync](../semantica/metadata-sync/index.md) endpoint |
    | You receive persons via Push | `POST /api/person-push` ([Person-Sync Push](../semantica/person-sync/push.md)) |
    | You download persons via Pull | None of your own, you only call DENA |

??? question "Can I integrate if my system is not Java?"

    Yes. DENA uses **standard REST + JSON**. Any language capable of making HTTP POST requests and returning JSON is compatible.

    There are [code snippets](../semantica/data-retrieve/snippets-codigo.md) in Java, C#, Python, Node.js and PHP.

??? question "What happens if I cannot implement the standard endpoint?"

    DENA will develop a **custom connector** adapted to your system (SOAP, files, proprietary API...).
    Contact the DENA team to coordinate it.

---

## Authentication

??? question "How do I obtain my OAuth2 credentials?"

    The credentials (`client_id` and `client_secret`) are provided by the DENA team during the onboarding process.
    They are specific to each administration and environment (PRE/PRO).

??? question "How long does the token last? Do I have to renew it?"

    Normally **5 minutes** (`expires_in: 300`). It is recommended to:

    - Cache the token while it is valid
    - Renew it ~60 seconds before expiry (leeway)
    - Not request a new token on every request

    [:octicons-arrow-right-24: get-token endpoint](../autenticacion/administracion-core-dena/endpoint-get-token.md)

??? question "Can I use my own IDP (Keycloak, ADFS, Cognito)?"

    Yes. When DENA calls your administration (Data-Retrieve), you provide the credentials and the URL of your IDP.
    DENA will use `client_credentials` to obtain the token before calling you.

    [:octicons-arrow-right-24: CORE DENA → Administration](../autenticacion/core-dena-administracion/index.md)

---

## Data-Retrieve

??? question "What do I return if I have no data for that person?"

    HTTP **200** with an empty `dataItems`:

    ```json
    {
      "context": { ... },
      "data": { "dataItems": [] },
      "code": "OK"
    }
    ```

    :material-alert: **Never** return 404 for "no data". 404 is only for "person does not exist in my system".

??? question "Can I return partial data if I only have some types?"

    Yes. Return only the objects you have available for the requested `dataTypeId`.
    If `RECORDS` is requested and you have no records for that person, return `dataItems: []`.

??? question "Are multilingual texts mandatory in Basque and Spanish?"

    It is **highly recommended** to include both languages (`SPANISH` + `BASQUE`).
    If you only have one language, include at least that one. The client app will display whatever is available.

??? question "How much time do I have to respond?"

    The standard timeout is **30 seconds**. If your system needs more time for complex queries,
    contact the DENA team to configure an extended timeout.

---

## Person-Sync

??? question "Pull or Push? Which one do I choose?"

    | Criterion | Pull | Push |
    |---|---|---|
    | You need real-time data | :material-close: | :material-check: |
    | Your system processes in batch | :material-check: | :material-close: |
    | You cannot expose endpoints | :material-check: | :material-close: |
    | You want to react immediately | :material-close: | :material-check: |

    You can use both simultaneously.

??? question "How often are Pull files generated?"

    Every **hour** an incremental file is generated with the changes since the last hour.
    You can also request custom exports with personalised filters.

---

## Connectivity and network

??? question "From which IPs does DENA call my system?"

    It depends on the environment. Contact the DENA team to obtain the IP range you must allow in your firewall.

??? question "Do I need an SSL certificate for my endpoint?"

    Yes. DENA only connects via **HTTPS**. Your endpoint must have a valid certificate
    (issued by a recognised CA or by the euskalsarea internal CA if you are on that network).

??? question "How do I validate that I have connectivity before go-live?"

    Use the [DENA Admin Connection Test]({{ repos.conx_test_tree }}) component.

    [:octicons-arrow-right-24: Communications guide](../guia-inicio/probar-comunicaciones.md)

---

## Tools

??? question "Can I test without a real system behind it?"

    Yes, use the [Records Mock](../guia-inicio/mock-expedientes.md) which simulates an administration with sample data.

??? question "Do you have Postman collections?"

    Yes. Available at [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

    They include requests for Data-Retrieve, Metadata-Sync, Person-Sync and authentication.

??? question "Is there a Swagger/OpenAPI for the DENA endpoints?"

    It is in the process of being published. In the meantime, the full specification is in this documentation.

---

## Support

??? question "Who do I contact if I have an integration problem?"

    **:material-email: DENA support email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    | Type of query | What to include |
    |---|---|
    | **Technical questions** | Problem description, relevant logs, environment (PRE/PRO) |
    | **Connectivity issues** | Tests performed, network configuration, error message |
    | **Credentials/Access** | Requesting administration, environment, client_id if you have it |
    | **Bugs/Issues** | Steps to reproduce, expected vs actual behaviour |
    
    :material-clock: **Response time:** Urgent queries during business hours are handled in less than 4 hours.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
