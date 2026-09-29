# :material-upload: PERSON-SYNC — PUSH

En el mecanismo **Push**, DENA notifica **proactivamente** a tu administración cada vez que una persona se registra, cambia sus datos o elimina su cuenta. Es el mecanismo recomendado cuando necesitas reaccionar **en tiempo real** a los cambios.

---

## ¿Cómo funciona?

En el Push, **DENA-CORE es el cliente** y **tu administración el servidor**: DENA hace un `POST` HTTP a un endpoint que tú expones, con los datos del cambio.

``` mermaid
sequenceDiagram
    participant DENA as CORE DENA (cliente)
    participant Admin as Tu administración (servidor)

    Note over DENA: Se registra persona nueva / cambio / baja
    DENA->>Admin: POST <tu-url-configurada> (datos del cambio + evento)
    Admin->>Admin: Procesa según el evento (alta / actualización / borrado)
    Admin-->>DENA: 200 OK
```

!!! important "DENA no impone una ruta"

    No existe un path predefinido para el endpoint de push. Tu administración lo expone en **la URL que configure en DENA** para su conector. DENA hará el `POST` contra esa URL. Lo único imprescindible es que acepte `POST` con `application/json` y devuelva el código HTTP correcto.

---

## ¿Qué necesita implementar la administración?

!!! info "Un endpoint que reciba el cambio y actúe según el evento"

    1. **Exponer un endpoint REST `POST`** que reciba el cuerpo JSON de la notificación.
    2. **Mirar el campo `syncEvent`** para saber qué ha pasado y actuar en consecuencia:

    | Evento | Acción |
    |--------|--------|
    | `CREATED` | Alta de la persona en tu copia local |
    | `UPDATED` | Actualización de los datos de la persona |
    | `ID_CHANGED` | Actualización del NIF/NIE (localizando por `oid`) |
    | `DELETED` | Borrado de la persona **y de sus datos asociados** |

    3. **Responder con el código HTTP** que refleje el resultado real (`200` si todo fue bien).

---

## ¿Por qué es importante mantener esta copia?

Tu administración solo debe enviar avisos de cambios ([Metadata-Sync / SRMD](../metadata-sync/index.md)) de personas que **realmente tienen cuenta en DENA**. El Push mantiene tu copia local al día para que:

- No envíes SRMD de personas que no están en DENA.
- Dejes de enviar SRMD de personas que se han dado de baja (evento `DELETED`).
- Dispongas al instante de los datos básicos (nombre, contacto) de las personas nuevas.

---

## Contrato del endpoint

La especificación completa —cuerpo de la petición, procesamiento por evento, respuesta, códigos HTTP y checklist de implementación— está en:

[:octicons-arrow-right-24: Endpoint Person Push to Admin](./endpoints/push/endpoint-person-push-to-admin.md)

---

!!! tip "Cuándo usar Push"

    - Cuando necesitas reaccionar **en tiempo real** a cambios de personas.
    - Cuando no quieres depender de ficheros periódicos ([Pull](./pull.md)).
    - Cuando tu sistema necesita el dato inmediatamente para poder notificar cambios vía Metadata-Sync.

    !!! note "Recomendación: Push + Pull"
        Se recomienda implementar **ambos** mecanismos: Push para el tiempo real y [Pull off-line](./pull.md) como respaldo para recuperar posibles notificaciones perdidas.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
