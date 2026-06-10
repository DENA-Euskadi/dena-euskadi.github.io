# Cliente ↔ CORE DENA

Para la autenticación entre la aplicación cliente y DENA, se utilizará el protocolo OAuth 2.0.

En primer lugar el cliente deberá autenticarse y obtener un token OAuth, para ello se ofrecen dos mecanismos: autenticación con Giltza o autenticación mediante WebAuthn. En el primer acceso se deberá realizar el proceso con Giltza, tras este se ofrecerá la creación de credenciales WebAuthn para los posteriores accesos.

El token OAuth obtenido deberá incluirse en las llamadas a DENA, mediante la cabecera `Authorization: Bearer <token>`

## Flujo de autenticación mediante Giltza

![Flujo autenticación Giltza](../adjuntos/imagenes/login-giltza.png)

Al acceder a la aplicación cliente, en primer lugar se autenticara al usuario con Giltza, obteniendo un token que será enviado a DENA, tras validar dicho token, DENA generará un token OAuth y lo enviara al cliente para las comunicaciones sucesivas.

## Flujo de registro mediante WebAuthn

Una vez autenticado con Giltza, es posible generar credenciales WebAuthn para los posteriores accesos, para ello se debe seguir el siguiente flujo:

![Flujo de registro WebAuthn](../adjuntos/imagenes/webauthn-register.png)

## Flujo de autenticación mediante WebAuthn

Con las credenciales generadas en el flujo de registro WebAuthn, la aplicación podrá obtener un token OAuth mediante el siguiente flujo:

![Flujo de autenticación WebAuthn](../adjuntos/imagenes/webauthn-login.png)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
