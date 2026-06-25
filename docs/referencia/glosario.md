# :material-book-alphabet: Glosario

Definición de los términos técnicos y funcionales utilizados en la documentación DENA.

---

## A

`API`
:   Application Programming Interface. Interfaz de programación de aplicaciones.

`ADR`
:   Architecture Decision Record. Registro formal de una decisión de arquitectura.

---

## C

`client_credentials`
:   Flujo OAuth2 donde un servicio se autentica con su `client_id` y `client_secret` para obtener un token. No requiere intervención de usuario.

`consentOid`
:   Identificador único del consentimiento otorgado por una persona para el acceso a sus datos.

`correlationId`
:   UUID que permite correlacionar todas las llamadas derivadas de una misma petición original.

---

## D

`Data-Retrieve`
:   Mecanismo mediante el cual DENA solicita datos de una persona a una administración.

`dataItems`
:   Array de objetos de dominio devueltos por la administración en la response de Data-Retrieve.

`DataTypeRef`
:   Objeto de referencia a un tipo de dato DENA (expediente, notificación, pago...).

`DENA`
:   Plataforma de interoperabilidad del Gobierno Vasco que facilita el acceso ciudadano a los datos de las administraciones.

`DIR3`
:   Directorio Común de Unidades Orgánicas y Oficinas. Catálogo oficial de la AGE que identifica unívocamente cada unidad administrativa.

---

## E

`EJIE`
:   Sociedad Informática del Gobierno Vasco. Entidad que gestiona la infraestructura TIC.

`Euskalsarea`
:   Red corporativa del Gobierno Vasco. Alternativa a Internet para comunicaciones internas.

---

## F

`flowDirection`
:   Campo que indica si un mensaje interop es una petición (`REQUEST`) o una respuesta (`RESPONSE`).

---

## G

`Giltza`
:   Sistema de autenticación del Gobierno Vasco basado en certificados digitales y cl@ve.

---

## I

`IDP`
:   Identity Provider. Proveedor de identidad que emite tokens (Keycloak, ADFS, Cognito...).

`interopRouteData`
:   Array de trazabilidad que registra por qué componentes DENA ha pasado un mensaje y en qué momento.

---

## J

`JWT`
:   JSON Web Token. Token firmado digitalmente utilizado para autenticación entre servicios.

---

## L

`LanguageTexts`
:   Objeto mapa clave-valor para textos multiidioma. Claves: `SPANISH`, `BASQUE`, `ENGLISH`.

`leeway`
:   Margen de seguridad (en segundos) aplicado antes de la expiración real de un token para evitar rechazos por latencia de red.

---

## M

`messageType`
:   Tipo de mensaje enviado en el campo `data` de un mensaje interop (ej: `PERSON_FETCH_DATA`, `CLIENT_LOGIN`).

`Metadata-Sync`
:   Mecanismo mediante el cual las administraciones notifican a DENA los cambios en datos de personas.

---

## N

`NIF`
:   Número de Identificación Fiscal. Identifica a personas físicas y jurídicas en España.

`NIE`
:   Número de Identidad de Extranjero. Identifica a residentes extranjeros en España.

---

## O

`OAuth2`
:   Open Authorization 2.0. Protocolo estándar de autorización utilizado para la comunicación entre servicios DENA.

`OID`
:   Object Identifier. Identificador técnico único asignado automáticamente por el sistema. Formato UUID.

`OrgAdminRef`
:   Objeto de referencia a una administración por su `oid` o `id`.

---

## P

`Person-Sync`
:   Mecanismo de sincronización del listado de personas registradas en DENA con las administraciones.

`PersonRef`
:   Objeto de referencia a una persona por su `oid` o `id` (NIF/NIE).

`Pull`
:   Modelo de sincronización donde la administración se conecta a DENA para descargar datos de personas.

`Push`
:   Modelo de sincronización donde DENA notifica proactivamente a la administración sobre cambios en personas.

---

## R

`R01F`
:   Framework base de desarrollo del Gobierno Vasco (Fabric). Proporciona utilidades, OIDs, serialización y servicios comunes.

`REST`
:   Representational State Transfer. Estilo de arquitectura para APIs web basado en HTTP.

---

## S

`SIA`
:   Sistema de Información Administrativa. Catálogo oficial de procedimientos y servicios de la AGE.

`subjectPerson`
:   Campo del contexto interop que identifica a la persona sobre la que se solicitan o envían datos.

---

## T

`Tanzú`
:   Plataforma de despliegue de contenedores (Kubernetes) utilizada por la infraestructura DENA.

---

## U

`UUID`
:   Universally Unique Identifier. Identificador de 128 bits en formato `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.

---

## W

`WebAuthn`
:   Web Authentication API. Estándar W3C para autenticación sin contraseña basada en criptografía asimétrica.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
