# :material-upload: PERSON-SYNC — PUSH

DENA notifica proactivamente a la administración cuando se registra una persona nueva o se producen cambios en sus datos.

---

## Flujo

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA
    participant Admin as Administración

    Note over DENA: Se registra persona nueva / cambio
    DENA->>Admin: POST /api/person-push (datos del cambio)
    Admin-->>DENA: 200 OK
```

---

## ¿Qué necesita la administración?

!!! info "Implementar un endpoint"

    La administración debe exponer un endpoint REST capaz de recibir los detalles del cambio.
    DENA invocará este endpoint cada vez que se produzca un alta o modificación de persona.

---

## Contrato del endpoint

[:octicons-arrow-right-24: Endpoint Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md) — Request, response, ejemplos JSON y códigos HTTP.

---

!!! tip "Cuándo usar Push"

    - Cuando necesitas reaccionar **en tiempo real** a cambios de personas
    - Cuando no quieres depender de ficheros periódicos (Pull)
    - Cuando tu sistema necesita el dato inmediatamente para notificar cambios vía Metadata-Sync

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
