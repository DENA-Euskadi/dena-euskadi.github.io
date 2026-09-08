# :material-frequently-asked-questions: Preguntas frecuentes (FAQ)

Respuestas a las dudas más comunes de las administraciones integradoras.

---

## Integración general

??? question "¿Qué necesito para empezar a integrarme con DENA?"

    1. **Credenciales OAuth2** proporcionadas por el equipo DENA (`client_id` + `client_secret`)
    2. **Conectividad HTTPS** hacia los endpoints de DENA (PRE y/o PRO)
    3. **Java 21+** y **Maven 3.9+** para compilar los proyectos de ejemplo/test

    [:octicons-arrow-right-24: Guía de instalación](../guia-inicio/instalacion.md)

??? question "¿Qué endpoints tengo que implementar obligatoriamente?"

    Depende de tu caso de uso:

    | Caso | Endpoint obligatorio |
    |---|---|
    | DENA consulta datos de tu administración | `POST /api/retrieveData` ([Data-Retrieve](../semantica/data-retrieve/index.md)) |
    | Notificas cambios a DENA | `POST` al endpoint DENA de [Metadata-Sync](../semantica/metadata-sync/index.md) |
    | Recibes personas por Push | `POST /api/person-push` ([Person-Sync Push](../semantica/person-sync/push.md)) |
    | Descargas personas por Pull | Ninguno propio, solo llamas a DENA |

??? question "¿Puedo integrarme si mi sistema no es Java?"

    Sí. DENA usa **REST + JSON estándar**. Cualquier lenguaje que pueda hacer HTTP POST y devolver JSON es compatible.

    Hay [snippets de código](../semantica/data-retrieve/snippets-codigo.md) en Java, C#, Python, Node.js y PHP.

??? question "¿Qué pasa si no puedo implementar el endpoint estándar?"

    DENA desarrollará un **conector a medida** que se adapte a tu sistema (SOAP, ficheros, API propietaria...).
    Contacta con el equipo DENA para coordinarlo.

---

## Autenticación

??? question "¿Cómo obtengo mis credenciales OAuth2?"

    Las credenciales (`client_id` y `client_secret`) las proporciona el equipo DENA durante el proceso de onboarding.
    Son específicas para cada administración y entorno (PRE/PRO).

??? question "¿Cuánto dura el token? ¿Tengo que renovarlo?"

    Normalmente **5 minutos** (`expires_in: 300`). Se recomienda:

    - Cachear el token mientras sea válido
    - Renovarlo ~60 segundos antes de expirar (leeway)
    - No solicitar un token nuevo en cada petición

    [:octicons-arrow-right-24: Endpoint get-token](../autenticacion/administracion-core-dena/index.md)

??? question "¿Puedo usar mi propio IDP (Keycloak, ADFS, Cognito)?"

    Sí. Cuando DENA llama a tu administración (Data-Retrieve), tú proporcionas las credenciales y la URL de tu IDP.
    DENA usará `client_credentials` para obtener el token antes de llamarte.

    [:octicons-arrow-right-24: CORE DENA → Administración](../autenticacion/core-dena-administracion/index.md)

??? question "¿Y si mi administración usa CAS u otro sistema que no es OAuth2?"

    No hay problema. DENA no impone OAuth2 como sistema de autenticación en vuestro lado. El flujo "DENA llama a tu sistema" (Data-Retrieve) se adapta al mecanismo que tu administración prefiera.

    Si usáis CAS, por ejemplo, el conector de DENA se configurará para autenticarse contra vuestro CAS antes de invocar vuestro endpoint.

    **Lo que necesitamos de tu lado:**

    - La URL de vuestro servicio CAS (endpoint para obtener el ticket/token de servicio)
    - Las credenciales (service account o equivalente) que DENA debe usar
    - Cómo incluir el ticket en las llamadas a vuestro data provider (header, parámetro, etc.)

    Con eso, el equipo DENA configura el conector para que se autentique contra vuestro sistema antes de cada llamada.

---

## Data-Retrieve

??? question "¿Qué devuelvo si no tengo datos para esa persona?"

    HTTP **200** con `dataItems` vacío:

    ```json
    {
      "context": { ... },
      "data": { "dataItems": [] },
      "code": "OK"
    }
    ```

    :material-alert: **Nunca** devuelvas 404 para "sin datos". El 404 es solo para "persona no existe en mi sistema".

??? question "¿Puedo devolver datos parciales si solo tengo algunos tipos?"

    Sí. Devuelve solo los objetos que tengas disponibles para el `dataTypeId` solicitado.
    Si te piden `RECORDS` y no tienes expedientes para esa persona, devuelve `dataItems: []`.

??? question "¿Los textos multiidioma son obligatorios en euskera y castellano?"

    Es **muy recomendable** incluir ambos idiomas (`SPANISH` + `BASQUE`).
    Si solo tienes un idioma, incluye al menos ese. La app cliente mostrará lo que haya disponible.

??? question "¿Cuánto tiempo tengo para responder?"

    El timeout estándar es **30 segundos**. Si tu sistema necesita más tiempo para consultas complejas,
    contacta con el equipo DENA para configurar un timeout extendido.

---

## Person-Sync

??? question "¿Pull o Push? ¿Cuál elijo?"

    | Criterio | Pull | Push |
    |---|---|---|
    | Necesitas datos en tiempo real | :material-close: | :material-check: |
    | Tu sistema procesa en batch | :material-check: | :material-close: |
    | No puedes exponer endpoints | :material-check: | :material-close: |
    | Quieres reaccionar inmediatamente | :material-close: | :material-check: |

    Puedes usar ambos simultáneamente.

??? question "¿Con qué frecuencia se generan los ficheros de Pull?"

    Cada **hora** se genera un fichero incremental con los cambios desde la última hora.
    También puedes solicitar exportaciones a medida con filtros personalizados.

---

## Conectividad y red

??? question "¿Desde qué IPs llama DENA a mi sistema?"

    Depende del entorno. Contacta con el equipo DENA para obtener el rango de IPs que debes permitir en tu firewall.

??? question "¿Necesito certificado SSL para mi endpoint?"

    Sí. DENA solo conecta por **HTTPS**. Tu endpoint debe tener un certificado válido
    (emitido por una CA reconocida o por la CA interna de euskalsarea si estás en esa red).

??? question "¿Cómo valido que tengo conectividad antes de la puesta en marcha?"

    Usa el componente [DENA Admin Connection Test]({{ repos.conx_test_tree }}).

    [:octicons-arrow-right-24: Guía de comunicaciones](../guia-inicio/probar-comunicaciones.md)

---

## Herramientas

??? question "¿Puedo probar sin un sistema real detrás?"

    Sí, usa el [Mock de Expedientes](../guia-inicio/mock-expedientes.md) que simula una administración con datos de ejemplo.

??? question "¿Tenéis colecciones Postman?"

    Sí. Disponibles en [`docs/adjuntos/postman/`]({{ repos.docs_tree }}/docs/adjuntos/postman).

    Incluyen requests para Data-Retrieve, Metadata-Sync, Person-Sync y autenticación.

??? question "¿Hay un Swagger/OpenAPI de los endpoints de DENA?"

    Está en proceso de publicación. Mientras tanto, la especificación completa está en esta documentación.

---

## Soporte

??? question "¿A quién contacto si tengo un problema de integración?"

    **:material-email: Email de soporte DENA:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    | Tipo de consulta | Qué incluir |
    |---|---|
    | **Dudas técnicas** | Descripción del problema, logs relevantes, entorno (PRE/PRO) |
    | **Problemas de conectividad** | Tests realizados, configuración de red, mensaje de error |
    | **Credenciales/Accesos** | Administración solicitante, entorno, client_id si lo tienes |
    | **Bugs/Issues** | Pasos para reproducir, comportamiento esperado vs real |
    
    :material-clock: **Tiempo de respuesta:** Consultas urgentes en horario laboral se atienden en menos de 4 horas.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
