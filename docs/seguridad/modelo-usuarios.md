# :material-account-key: Modelo de usuarios del security context

Cuando una petición llega a DENA-CORE, la parte que la origina queda representada por un **usuario** dentro del **security context**. DENA distingue varios tipos de usuario según quién actúe: una persona, un usuario interno de gestión o un sistema de una administración.

Todos comparten una interfaz de marca común, `DN00IsDENAUser`, que permite usarlos de forma homogénea dentro del security context.

---

## Tipos de usuario

| Tipo | Clase | Representa | Identificador principal |
|------|-------|-----------|-------------------------|
| Persona | `DN00DENAPersonUser` | Una persona autenticada a través de un proveedor de identidad | Referencia a la persona + ID en el Identity Broker |
| Gestión | `DN00DENAManagementUser` | Un usuario interno de gestión | Código de puesto de trabajo |
| Sistema de administración | `DN00DENAAdminSystemUser` | Una administración que actúa como sistema (sin persona detrás) | Referencia a la administración |

---

## Usuario Persona

`DN00DENAPersonUser` (`personUser` en JSON) actúa como **puente de identidad** entre una persona y su usuario técnico en el Identity Broker (Keycloak, WSO2...).

Es importante no confundir los conceptos:

- Una **persona** es el individuo que tiene una cuenta en el sistema.
- Un **usuario** es la identidad técnica de seguridad que gestiona el Identity Broker.

El usuario persona enlaza ambos con un conjunto mínimo de datos:

| Campo | JSON | Descripción |
|-------|------|-------------|
| Referencia de persona | `personRef` | OID de la persona en la base de datos de negocio |
| ID del usuario en el broker | `personUserId` | ID asignado por el Identity Broker (p. ej. el `user_id` de Keycloak) |
| Nombre | `givenName` | Nombre de la persona |
| Apellidos | `familyName` | Apellidos de la persona |

!!! info "Flujo de identidad"
    La persona se autentica en un proveedor de identidad (por ejemplo GILTZA con su DNI). El Identity Broker localiza o crea el usuario correspondiente y lo asocia a una sesión. Con la referencia de persona (`personRef`), la aplicación sabe exactamente a qué persona física corresponde el usuario, y puede cargar sus roles y datos de negocio.

---

## Usuario de Gestión

`DN00DENAManagementUser` (`internalUser` en JSON) representa a un **usuario interno de gestión**.

| Campo | JSON | Descripción |
|-------|------|-------------|
| Código de puesto | `workPlaceCode` | Puesto de trabajo asociado al usuario |

---

## Usuario de Sistema de Administración

`DN00DENAAdminSystemUser` (`adminSystemUser` en JSON) representa a una **administración que actúa como sistema**, es decir, cuando quien opera no es una persona sino la propia administración (por ejemplo, en llamadas máquina a máquina con `client_credentials`).

| Campo | JSON | Descripción |
|-------|------|-------------|
| Administración | `admin` | Referencia a la administración (`DN00OrgAdminRef`) |

Dos usuarios de sistema se consideran el mismo sujeto cuando referencian la misma administración (por OID o por ID). El nombre para mostrar del usuario es el de la administración referenciada.

!!! note "Relación con las áreas de autenticación"
    Este tipo de usuario encaja con el **Área 3** de autenticación (una administración se autentica en DENA-CORE mediante `client_credentials`). Consulta [Seguridad y Autenticación](index.md) para el detalle de las áreas.

---

## Validación

Los tres tipos de usuario se autovalidan mediante `DN00DENAUserValidator`, que aplica las reglas de validación comunes al modelo de usuario.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
