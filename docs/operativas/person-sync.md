# :material-account-sync: Person-Sync (Sincronizar Personas)

## Concepto

Person-Sync es la operativa que permite a tu administración **saber qué personas están inscritas en DENA**. Esto es necesario para que tu admin solo envíe SRMD (avisos de cambios) para personas que realmente tienen una cuenta DENA.

Además, DENA comparte con tu admin datos básicos de la persona: NIF, nombre, apellidos, datos de contacto.

![Person-Sync Overview](../../adjuntos/imagenes/image18.png)

### Datos de Persona Sincronizados

Cada persona que se registra en DENA tiene una cuenta que almacena:

- **OID de Persona**: Identificador único generado por DENA
- **ID de Persona**: El NIF
- **Nombre Completo**: nombre, apellido1, apellido2
- **Datos de Contacto**: dirección, teléfono, email...
- **Temas Preferidos**: pueden usarse en servicios proactivos o personalizar la UI

### ¿Para qué sirve?

- **Saber para quién enviar SRMD**: solo tiene sentido notificar cambios de personas que están en DENA
- **Mantener tu BD sincronizada**: cuando una persona se inscribe o se da de baja de DENA, tu admin se entera
- **Acceder a datos básicos**: nombre, contacto, preferencias temáticas

---

## Mecanismos disponibles

DENA ofrece dos mecanismos complementarios:

### Push: DENA te notifica

DENA envía un mensaje a un servicio de tu admin cada vez que:

- Una nueva persona se inscribe en DENA
- Una persona elimina su cuenta
- Una persona cambia sus datos básicos (nombre, contacto...)

Tu admin expone un endpoint que DENA llama automáticamente cuando hay cambios.

### Pull: tu admin consulta

Tu admin consulta a DENA cuando quiere, de dos formas:

| Modalidad | Descripción |
|-----------|-------------|
| **On-line** | Tu admin llama a un REST service de DENA para consultar personas en tiempo real |
| **Off-line (pre-generado)** | DENA genera fichero periódicos con el listado de personas |
| **Off-line (bespoke)** | Tu admin solicita un ficheo personalizado que DENA genera bajo demanda |

![Person-Sync Pull Flow](../../adjuntos/imagenes/person-sync-pull.png)

---

## ¿Cuál elegir?

| Situación | Recomendación |
|-----------|---------------|
| Necesitas saber al instante cuando alguien se inscribe | **Push** |
| Tienes un proceso batch nocturno que sincroniza | **Pull off-line** |
| Quieres consultar personas bajo demanda | **Pull on-line** |
| Quieres ambos (recomendado) | **Push** + **Pull off-line** como respaldo |

!!! tip "Recomendación"
    Se recomienda implementar **ambos mecanismos**: Push para notificaciones en tiempo real y Pull off-line como respaldo para recuperar posibles perdidos.

---

## Especificación completa

Para la especificación detallada de endpoints, modelos y ficheos:

| Mecanismo | Documentación |
|-----------|---------------|
| Push | [:octicons-arrow-right-24: Endpoint Push](../semantica/person-sync/push.md) |
| Pull | [:octicons-arrow-right-24: Endpoint Pull](../semantica/person-sync/pull.md) |
| Visión general | [:octicons-arrow-right-24: Semántica Person-Sync](../semantica/person-sync/index.md) |

---

**Siguiente:** [:octicons-arrow-right-24: Semántica (especificación técnica de datos)](../semantica/index.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
