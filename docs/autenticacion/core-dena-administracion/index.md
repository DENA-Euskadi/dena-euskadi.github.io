# CORE DENA → Administración

La autenticación estandar cuando DENA realice una llamada a una administración será OAuth 2.0.

La administración deberá proveer a DENA unas credenciales (client-id y client-secret) que DENA utilizara para obtener un token mediante un flujo client-credentials.

Posteriormente se utilizará el token obtenido en las llamadas a la administración, incluyendolo en una cabecera `Authorization: Bearer <token>`.

## Contenido

- [Modelo](modelo.md)
- [Servicios](servicios.md)










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
