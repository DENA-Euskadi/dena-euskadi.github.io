# :material-checkbox-multiple-marked: Onboarding Checklist

Step-by-step guide from "I've been given access" to "my integration is working".

---

## :material-numeric-1-circle: Prepare the environment

- [ ] Install JDK 21+
- [ ] Install Maven 3.9+
- [ ] Configure `settings.xml` with the DENA repositories
- [ ] Clone the connectivity test repository

[:octicons-arrow-right-24: Installation guide](./instalacion.md)

---

## :material-numeric-2-circle: Obtain credentials

- [ ] Request `client_id` and `client_secret` from the DENA team
- [ ] Receive the token endpoint URL (Keycloak DENA)
- [ ] Confirm the assigned environment (PRE/PRO)

!!! info "Contact for credentials and support"

    **:material-email: DENA Contact:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    Credentials are provided by the DENA team during the administration onboarding phase. For any questions about the onboarding process or technical issues, contact the support team.

---

## :material-numeric-3-circle: Validate connectivity

- [ ] Deploy DENA Admin Connection Test
- [ ] Verify **Admin → DENA** (test against PRE)
- [ ] Coordinate with infrastructure so that **DENA → Admin** works
- [ ] Obtain confirmation of bidirectional connectivity

[:octicons-arrow-right-24: Communications guide](./probar-comunicaciones.md)

---

## :material-numeric-4-circle: Implement the endpoint

- [ ] Choose what to implement first (usually Data-Retrieve)
- [ ] Read the endpoint specification
- [ ] Implement `POST /api/retrieveData` in your system
- [ ] Return at least one data type (e.g.: records)
- [ ] Include multilingual texts (SPANISH + BASQUE)
- [ ] Respect standard HTTP codes

[:octicons-arrow-right-24: Data-Retrieve implementation guide](../semantica/data-retrieve/guia-implementacion.md)

---

## :material-numeric-5-circle: Test with the mock

- [ ] Deploy the records mock
- [ ] Verify that the demo1 connector connects to your endpoint
- [ ] Test with different `personId` and `dataTypeId`
- [ ] Verify the response is in the correct format

[:octicons-arrow-right-24: Records Mock](./mock-expedientes.md)

---

## :material-numeric-6-circle: Test authentication

- [ ] Obtain a token with your credentials
- [ ] Include the token in calls to DENA
- [ ] If DENA calls you: configure your IDP and provide credentials to DENA
- [ ] Verify that authenticated calls work

[:octicons-arrow-right-24: Authentication](../autenticacion/index.md)

---

## :material-numeric-7-circle: Validate end-to-end in PRE

- [ ] DENA invokes your real endpoint with a test person
- [ ] Verify that the response reaches the CORE correctly
- [ ] Test Metadata-Sync if applicable
- [ ] Test Person-Sync if applicable

---

## :material-numeric-8-circle: Move to PRO

- [ ] Request PRO credentials
- [ ] Verify connectivity to PRO endpoints
- [ ] Repeat end-to-end test in PRO
- [ ] Confirm production deployment with the DENA team

---

!!! success "Integration completed!"

    Once all steps are passed, your administration will be integrated with DENA and users will be able to access their data from the app.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
