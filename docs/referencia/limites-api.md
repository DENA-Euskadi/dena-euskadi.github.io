# :material-speedometer: Limites y Restricciones de la API

Parametros operativos conocidos de la API DENA.

---

## Timeouts

| Operacion | Timeout | Referencia |
|-----------|---------|------------|
| **Data-Retrieve** (DENA → tu admin) | 30 segundos | Campo `protocol.timeOut: "30s"` en la request |

!!! tip "Data-Retrieve: 30 segundos"
    Tu endpoint debe responder en menos de 30 segundos. Si necesitas mas tiempo por consultas complejas, contacta al equipo DENA para configurar un timeout extendido para tu conector.

---

## Token OAuth2

| Parametro | Valor | Referencia |
|-----------|-------|------------|
| **Duracion del token** | 300 segundos (5 min) | Campo `expires_in` en la respuesta del endpoint get-token |
| **Leeway recomendado** | ~60 segundos antes de expiracion | Recomendacion documentada en la seccion Tu Sistema Llama a DENA y FAQ |
| **Grant type** | `client_credentials` | Unico grant type soportado |

---

## Valores pendientes de documentar

!!! warning "Pendiente de definicion"
    Los siguientes parametros operativos no estan documentados actualmente. Contacta al equipo DENA si necesitas esta informacion para tu integracion:
    
    - **Rate limits**: numero maximo de peticiones por minuto/hora por administracion
    - **Tamano maximo de payload**: limite de tamano del body en requests y responses
    - **Politica de reintentos**: comportamiento de DENA ante errores de tu endpoint
    - **SLA de disponibilidad**: porcentaje de uptime garantizado por entorno (PRE/PRO)
    - **Versionado de la API**: politica de compatibilidad y comunicacion de breaking changes
    - **Ventanas de mantenimiento**: horarios planificados de indisponibilidad

---

## Recomendaciones generales

Basadas en buenas practicas y en el comportamiento documentado del sistema:

- **Cachea el token** mientras sea valido. No solicites un token nuevo en cada peticion.
- **Renueva el token** con anticipacion (~60s antes de expirar) para evitar rechazos por latencia de red.
- **Responde rapido**: el timeout de 30s es el maximo, pero cuanto mas rapido respondas, mejor experiencia para la persona ciudadana.
- **Devuelve siempre HTTP 200** con `dataItems: []` cuando no hay datos. No uses 404.

---

!!! question "Necesitas informacion sobre limites?"
    
    Para consultas sobre rate limits, tamanos maximos de payload o SLA:
    
    **:material-email:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
