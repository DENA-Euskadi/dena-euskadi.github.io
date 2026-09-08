# :material-account-key: Security context user model

When a request reaches DENA-CORE, the originating party is represented by a **user** within the **security context**. DENA distinguishes several user types depending on who acts: a person, an internal management user, or an administration acting as a system.

They all share a common marker interface, `DN00IsDENAUser`, which allows them to be used uniformly within the security context.

---

## User types

| Type | Class | Represents | Main identifier |
|------|-------|-----------|-----------------|
| Person | `DN00DENAPersonUser` | A person authenticated through an identity provider | Person reference + ID in the Identity Broker |
| Management | `DN00DENAManagementUser` | An internal management user | Work place code |
| Administration system | `DN00DENAAdminSystemUser` | An administration acting as a system (no person behind it) | Administration reference |

---

## Person user

`DN00DENAPersonUser` (`personUser` in JSON) acts as an **identity bridge** between a person and their technical user in the Identity Broker (Keycloak, WSO2...).

It is important not to conflate the concepts:

- A **person** is the individual who has an account in the system.
- A **user** is the technical security identity managed by the Identity Broker.

The person user links both with a minimal set of data:

| Field | JSON | Description |
|-------|------|-------------|
| Person reference | `personRef` | OID of the person in the business database |
| Broker user ID | `personUserId` | ID assigned by the Identity Broker (e.g. Keycloak `user_id`) |
| First name | `givenName` | Person's first name |
| Last name | `familyName` | Person's last name |

!!! info "Identity flow"
    The person authenticates against an identity provider (for example GILTZA with their national ID). The Identity Broker locates or provisions the matching user and attaches it to a session. With the person reference (`personRef`), the application knows exactly which physical person the user corresponds to, and can load their roles and business data.

---

## Management user

`DN00DENAManagementUser` (`internalUser` in JSON) represents an **internal management user**.

| Field | JSON | Description |
|-------|------|-------------|
| Work place code | `workPlaceCode` | Work place associated with the user |

---

## Administration system user

`DN00DENAAdminSystemUser` (`adminSystemUser` in JSON) represents an **administration acting as a system**, i.e. when the operating party is not a person but the administration itself (for example, in machine-to-machine calls using `client_credentials`).

| Field | JSON | Description |
|-------|------|-------------|
| Administration | `admin` | Administration reference (`DN00OrgAdminRef`) |

Two system users are considered the same subject when they reference the same administration (by OID or by ID). The user's display name is that of the referenced administration.

!!! note "Relation to authentication areas"
    This user type fits **Area 3** of authentication (an administration authenticates against DENA-CORE via `client_credentials`). See [Security & Authentication](index.md) for the area details.

---

## Validation

All three user types self-validate through `DN00DENAUserValidator`, which applies the common validation rules for the user model.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
