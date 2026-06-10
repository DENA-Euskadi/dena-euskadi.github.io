# 🔧 DevTools

Herramientas de desarrollo que facilitan las pruebas e integración con DENA para administraciones públicas.

---

## Herramientas disponibles

### DENA Admin Connection Test

**Repositorio:** [DENA-Euskadi/dena-admin-conx-test](https://github.com/DENA-Euskadi/dena-admin-conx-test)

Herramienta que permite a los desarrolladores de las administraciones realizar **conexiones de prueba** con DENA para validar:

- ✅ Conectividad con los endpoints de DENA
- ✅ Configuración de autenticación OAuth2  
- ✅ Formato y estructura de requests/responses
- ✅ Validación de certificados y configuración de red
- ✅ Pruebas de endpoints específicos (data-retrieve, metadata-sync, person-sync)

### Casos de uso

Esta herramienta es especialmente útil para:

- **Desarrolladores** que implementan la integración con DENA
- **Administradores de sistemas** que configuran la conectividad
- **Equipos de QA** que validan las integraciones
- **Soporte técnico** para diagnóstico de problemas de conectividad

### Características

- 🚀 **Fácil configuración** — Configuración mediante archivos de propiedades
- 🔒 **Soporte OAuth2** — Autenticación automática con DENA
- 📊 **Logging detallado** — Información completa de requests/responses
- 🔄 **Múltiples endpoints** — Pruebas de diferentes APIs de DENA
- 🛠️ **Troubleshooting** — Ayuda en la identificación de problemas

---

## Requisitos

- **Java 21+**
- **Credenciales OAuth2** proporcionadas por DENA
- **Conectividad HTTPS** con los entornos de DENA

---

## Uso recomendado

1. **Fase de desarrollo** — Validar la integración durante el desarrollo
2. **Pruebas de conectividad** — Verificar la configuración de red
3. **Troubleshooting** — Diagnosticar problemas de integración
4. **Validación de endpoints** — Confirmar el formato de datos

---

## Soporte

Para soporte técnico o incidencias con las herramientas de desarrollo:

- 📧 **Email técnico:** [Contacto DENA]
- 📖 **Documentación:** [Esta documentación](../index.md)
- 🛠️ **Issues:** Reportar en el repositorio correspondiente


<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>