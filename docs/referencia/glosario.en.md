# :material-book-alphabet: Glossary

Definition of technical and functional terms used in the DENA documentation.

---

## A

`API`
:   Application Programming Interface.

`ADR`
:   Architecture Decision Record. Formal record of an architecture decision.

---

## C

`client_credentials`
:   OAuth2 flow where a service authenticates with its `client_id` and `client_secret` to obtain a token. No user intervention required.

`consentOid`
:   Unique identifier of the consent granted by a person for access to their data.

`correlationId`
:   UUID that allows correlating all calls derived from the same original request.

---

## D

`Data-Retrieve`
:   Mechanism through which DENA requests data about a person from an administration.

`dataItems`
:   Array of domain objects returned by the administration in the Data-Retrieve response.

`DataTypeRef`
:   Reference object to a DENA data type (record, notification, payment...).

`DENA`
:   Interoperability platform of the Basque Government that facilitates citizen access to administration data.

`DIR3`
:   Common Directory of Organizational Units and Offices. Official catalogue of the Spanish Central Administration that uniquely identifies each administrative unit.

---

## E

`EJIE`
:   IT Company of the Basque Government. Entity that manages the ICT infrastructure.

`Euskalsarea`
:   Corporate network of the Basque Government. Alternative to the Internet for internal communications.

---

## F

`flowDirection`
:   Field that indicates whether an interop message is a request (`REQUEST`) or a response (`RESPONSE`).

---

## G

`Giltza`
:   Authentication system of the Basque Government based on digital certificates and cl@ve.

---

## I

`IDP`
:   Identity Provider. Identity provider that issues tokens (Keycloak, ADFS, Cognito...).

`interopRouteData`
:   Traceability array that records which DENA components a message has passed through and at what time.

---

## J

`JWT`
:   JSON Web Token. Digitally signed token used for authentication between services.

---

## L

`LanguageTexts`
:   Key-value map object for multilingual texts. Keys: `SPANISH`, `BASQUE`, `ENGLISH`.

`leeway`
:   Safety margin (in seconds) applied before the actual expiration of a token to avoid rejections due to network latency.

---

## M

`messageType`
:   Type of message sent in the `data` field of an interop message (e.g.: `PERSON_FETCH_DATA`, `CLIENT_LOGIN`).

`Metadata-Sync`
:   Mechanism through which administrations notify DENA of changes in person data.

---

## N

`NIF`
:   Tax Identification Number. Identifies natural and legal persons in Spain.

`NIE`
:   Foreigner Identity Number. Identifies foreign residents in Spain.

---

## O

`OAuth2`
:   Open Authorization 2.0. Standard authorization protocol used for communication between DENA services.

`OID`
:   Object Identifier. Unique technical identifier automatically assigned by the system. UUID format.

`OrgAdminRef`
:   Reference object to an administration by its `oid` or `id`.

---

## P

`Person-Sync`
:   Synchronization mechanism for the list of persons registered in DENA with the administrations.

`PersonRef`
:   Reference object to a person by their `oid` or `id` (NIF/NIE).

`Pull`
:   Synchronization model where the administration connects to DENA to download person data.

`Push`
:   Synchronization model where DENA proactively notifies the administration about changes in persons.

---

## R

`R01F`
:   Base development framework of the Basque Government (Fabric). Provides utilities, OIDs, serialization and common services.

`REST`
:   Representational State Transfer. Architecture style for web APIs based on HTTP.

---

## S

`SIA`
:   Administrative Information System. Official catalogue of procedures and services of the Spanish Central Administration.

`subjectPerson`
:   Interop context field that identifies the person about whom data is requested or sent.

---

## T

`Tanzú`
:   Container deployment platform (Kubernetes) used by the DENA infrastructure.

---

## U

`UUID`
:   Universally Unique Identifier. 128-bit identifier in `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` format.

---

## W

`WebAuthn`
:   Web Authentication API. W3C standard for passwordless authentication based on asymmetric cryptography.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
