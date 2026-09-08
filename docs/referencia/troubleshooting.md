# :material-lifebuoy: Troubleshooting

Guía centralizada de errores comunes y su resolución, organizada por categoría.

---

## Conectividad

??? failure "Connection refused"

    **Síntoma:** No se puede establecer conexión con el endpoint.

    **Causas posibles:**

    - Servicio no arrancado
    - Puerto cerrado por firewall
    - IP/hostname incorrecto

    **Solución:**

    ```bash
    # Verificar que el servicio está corriendo
    curl -v http://localhost:8082/api/hello

    # Verificar conectividad de red
    telnet api-batera.pre.dena.eus 443

    # Verificar resolución DNS
    nslookup api-batera.pre.dena.eus
    ```

??? failure "Connection timeout"

    **Síntoma:** La conexión se inicia pero no se completa en el tiempo esperado.

    **Causas posibles:**

    - Firewall bloqueando silenciosamente
    - Proxy corporativo no configurado
    - Reglas de red no abiertas

    **Solución:**

    - Verificar reglas de firewall con el equipo de infraestructura
    - Configurar proxy: `export https_proxy=http://proxy:3128`
    - Solicitar apertura de tráfico hacia las IPs de DENA

??? failure "SSL/TLS handshake error"

    **Síntoma:** `javax.net.ssl.SSLHandshakeException` o similar.

    **Causas posibles:**

    - Certificado del servidor no reconocido por el truststore de Java
    - Versión TLS incompatible
    - Certificado expirado

    **Solución:**

    ```bash
    # Descargar el certificado
    openssl s_client -connect api-batera.pre.dena.eus:443 < /dev/null 2>/dev/null | \
      openssl x509 > dena-ca.crt

    # Importar al truststore
    keytool -import -trustcacerts -alias dena-ca \
      -file dena-ca.crt \
      -keystore $JAVA_HOME/lib/security/cacerts \
      -storepass changeit
    ```

---

## Autenticación

??? failure "401 Unauthorized"

    **Síntoma:** `{"error": "invalid_token"}` o respuesta HTTP 401.

    **Causas posibles:**

    - Token expirado
    - Token de entorno incorrecto (token de PRE usado en PRO)
    - Token mal formado en la cabecera

    **Solución:**

    - Verificar que el token no ha expirado (`expires_in`)
    - Regenerar el token con el endpoint correcto
    - Verificar formato de cabecera: `Authorization: Bearer <token>` (con espacio después de Bearer)

??? failure "invalid_client al obtener token"

    **Síntoma:** `{"error": "invalid_client", "error_description": "Invalid client or Invalid client credentials"}`

    **Causas posibles:**

    - `client_id` incorrecto
    - `client_secret` incorrecto
    - Credenciales de un entorno distinto

    **Solución:**

    - Verificar `client_id` y `client_secret` (sin espacios en blanco)
    - Confirmar que las credenciales son del entorno correcto (PRE vs PRO)
    - Contactar al equipo DENA si las credenciales no funcionan

??? failure "403 Forbidden"

    **Síntoma:** Token válido pero sin permisos.

    **Causas posibles:**

    - El cliente no tiene los scopes/roles necesarios
    - El recurso requiere permisos adicionales

    **Solución:**

    - Contactar al equipo DENA para revisar los permisos del cliente

---

## Data-Retrieve

??? failure "400 Bad Request — campos obligatorios"

    **Síntoma:** `{"code": "CLIENT_ERR", "details": "Missing required fields..."}`

    **Causas posibles:**

    - Falta `context.message.type`
    - Falta `context.subjectPerson.id`
    - Falta `context.dataType.id`
    - Falta `context.message.correlationId`

    **Solución:**

    Verificar que el JSON incluye todos los campos obligatorios:

    ```json
    {
      "context": {
        "message": {
          "type": "PERSON_FETCH_DATA",
          "correlationId": "uuid-here"
        },
        "dataType": { "id": "administrativeServiceProcedureRecord", "oid": "DTYPE-OID-RECORDS" },
        "subjectPerson": { "id": "12345678A", "oid": "PERSON-OID-001" }
      },
      "payload": {}
    }
    ```

??? failure "Respuesta vacía (dataItems: [])"

    **Síntoma:** HTTP 200 pero sin datos.

    **Causas posibles:**

    - La persona no tiene datos del tipo solicitado
    - El `subjectPerson.id` no existe en el sistema de la administración
    - El `dataType.id` no es reconocido

    **Solución:**

    - Verificar que el `subjectPerson.id` existe en tu sistema
    - Verificar que el `dataType.id` coincide con los tipos que tu administración gestiona
    - Esto puede ser un comportamiento correcto (persona sin expedientes, por ejemplo)

??? failure "Timeout en la respuesta"

    **Síntoma:** DENA recibe timeout al llamar a tu endpoint.

    **Causas posibles:**

    - Consulta a base de datos lenta
    - Servicio sobrecargado
    - Timeout configurado demasiado bajo

    **Solución:**

    - Optimizar las consultas a base de datos
    - El timeout estándar es 30s. Responder siempre dentro de ese margen
    - Si necesitas más tiempo, contacta al equipo DENA

---

## Traffic Flow (cabeceras DENA)

??? failure "Missing required context fields"

    **Síntoma:** `{"status": 400, "message": "Missing required context fields: context.message.type..."}`

    **Causas posibles:**

    - El body no contiene el campo `context`
    - Faltan campos obligatorios dentro de `context`

    **Solución:**

    Campos obligatorios en el body:

    - `context.message.type`
    - `context.message.correlationId`
    - `context.message.interopRouteData`
    - `context.originClientInstallment` u `context.originAdmin` (según el origen del mensaje)

??? failure "Hash mismatch (X-DENA-Data-Digest)"

    **Síntoma:** Rechazo por hash incorrecto en cabeceras DENA.

    **Causas posibles:**

    - El body fue modificado después de calcular el hash
    - Proxy/middleware alterando el body
    - Encoding incorrecto (UTF-8 esperado)

    **Solución:**

    - Calcular el hash sobre el body exacto que se envía por red
    - No modificar el body después de generar el digest
    - Verificar que no hay proxies intermedios alterando contenido

---

## Compilación y despliegue

??? failure "BUILD FAILURE — dependencias no resueltas"

    **Síntoma:** Maven no encuentra artefactos DENA/R01F.

    **Solución:**

    Verificar `settings.xml`:

    ```xml
    <repository>
      <id>ejie-group</id>
      <url>https://bin.alm80.itbatera.euskadi.eus/repository/in-house-80-app-group/</url>
    </repository>
    ```

??? failure "java.lang.UnsupportedClassVersionError"

    **Síntoma:** Error al ejecutar con versión de Java incorrecta.

    **Solución:**

    ```bash
    # Verificar versión
    java -version  # Debe ser 21+

    # Verificar JAVA_HOME
    echo $JAVA_HOME
    ```

---

!!! tip "¿No encuentras tu error?"

    Si el problema persiste o no encuentras tu error específico en esta guía:
    
    **:material-email: Contacta al equipo de soporte DENA:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    Incluye en tu consulta:
    
    - Mensaje de error completo
    - Logs relevantes
    - Entorno (PRE/PRO/local)
    - `message.correlationId` si lo tienes
    - Información de contexto (qué estabas intentando hacer)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
