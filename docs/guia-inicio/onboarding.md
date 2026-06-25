# :material-checkbox-multiple-marked: Checklist de Onboarding

Guía paso a paso desde "me han dado acceso" hasta "tengo mi integración funcionando".

---

## :material-numeric-1-circle: Preparar el entorno

- [ ] Instalar JDK 21+
- [ ] Instalar Maven 3.9+
- [ ] Configurar `settings.xml` con los repositorios DENA
- [ ] Clonar el repositorio de test de conectividad

[:octicons-arrow-right-24: Guía de instalación](./instalacion.md)

---

## :material-numeric-2-circle: Obtener credenciales

- [ ] Solicitar `client_id` y `client_secret` al equipo DENA
- [ ] Recibir la URL del token endpoint (Keycloak DENA)
- [ ] Confirmar el entorno asignado (PRE/PRO)

!!! info "Contacto para credenciales y soporte"

    **:material-email: Contacto DENA:** [admin-digital-data-dena@ejie.eus](mailto:admin-digital-data-dena@ejie.eus)
    
    Las credenciales las proporciona el equipo DENA durante la fase de alta de la administración. Para cualquier duda sobre el proceso de onboarding o problemas técnicos, contacta con el equipo de soporte.

---

## :material-numeric-3-circle: Validar conectividad

- [ ] Desplegar DENA Admin Connection Test
- [ ] Verificar **Admin → DENA** (test contra PRE)
- [ ] Coordinar con infraestructura para que **DENA → Admin** funcione
- [ ] Obtener confirmación de conectividad bidireccional

[:octicons-arrow-right-24: Guía de comunicaciones](./probar-comunicaciones.md)

---

## :material-numeric-4-circle: Implementar el endpoint

- [ ] Elegir qué implementar primero (normalmente Data-Retrieve)
- [ ] Leer la especificación del endpoint
- [ ] Implementar el `POST /api/retrieveData` en tu sistema
- [ ] Devolver al menos un tipo de dato (ej: expedientes)
- [ ] Incluir textos multiidioma (SPANISH + BASQUE)
- [ ] Respetar los códigos HTTP estándar

[:octicons-arrow-right-24: Guía de implementación Data-Retrieve](../semantica/data-retrieve/guia-implementacion.md)

---

## :material-numeric-5-circle: Probar con el mock

- [ ] Desplegar el mock de expedientes
- [ ] Verificar que el conector demo1 conecta con tu endpoint
- [ ] Probar con diferentes `personId` y `dataTypeId`
- [ ] Verificar la respuesta en formato correcto

[:octicons-arrow-right-24: Mock de Expedientes](./mock-expedientes.md)

---

## :material-numeric-6-circle: Probar autenticación

- [ ] Obtener un token con tus credenciales
- [ ] Incluir el token en las llamadas a DENA
- [ ] Si DENA te llama: configurar tu IDP y proveer credenciales a DENA
- [ ] Verificar que las llamadas autenticadas funcionan

[:octicons-arrow-right-24: Autenticación](../autenticacion/index.md)

---

## :material-numeric-7-circle: Validar end-to-end en PRE

- [ ] DENA invoca tu endpoint real con una persona de prueba
- [ ] Verificar que la respuesta llega correctamente al CORE
- [ ] Probar Metadata-Sync si aplica
- [ ] Probar Person-Sync si aplica

---

## :material-numeric-8-circle: Paso a PRO

- [ ] Solicitar credenciales de PRO
- [ ] Verificar conectividad hacia endpoints PRO
- [ ] Repetir test end-to-end en PRO
- [ ] Confirmar con el equipo DENA la puesta en producción

---

!!! success "¡Integración completada!"

    Una vez superados todos los pasos, tu administración estará integrada con DENA y las personas usuarias podrán acceder a sus datos desde la app.

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
