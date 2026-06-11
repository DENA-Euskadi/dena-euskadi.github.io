# 🔧 DevTools

Herramientas de desarrollo que facilitan las pruebas e integración con DENA para administraciones públicas.

---

## Herramientas disponibles

### DENA DevTools Services

**Entornos disponibles:**
- 🌐 **PRE (Internet):** [https://api-batera.pre.dena.eus/devtools-services/](https://api-batera.pre.dena.eus/devtools-services/)
- 🌐 **PRE (Euskalsarea):** [https://api-batera.pre.batera.euskalsarea.eus/devtools-services/](https://api-batera.pre.batera.euskalsarea.eus/devtools-services/)
- 🔒 **PRO (Internet):** [https://api-batera.pro.dena.eus/devtools-services/](https://api-batera.pro.dena.eus/devtools-services/) *(próximamente)*
- 🔒 **PRO (Euskalsarea):** [https://api-batera.pro.batera.euskalsarea.eus/devtools-services/](https://api-batera.pro.batera.euskalsarea.eus/devtools-services/) *(próximamente)*

**Herramienta web para testing de endpoints HTTP** que permite a las administraciones y desarrolladores probar conectividad y realizar llamadas HTTP desde la infraestructura DENA:

#### 🎯 Funcionalidades principales

- 🔗 **Test de endpoints HTTP** - Llamadas GET, POST, PUT, DELETE, PATCH a cualquier URL
- 🔑 **Soporte de autenticación** - Bearer Token, Basic Auth, API Key
- 📤 **Múltiples tipos de body** - JSON, Form Data, URL Encoded, Raw Text
- 📋 **Headers personalizables** - Configuración completa de headers HTTP
- 🔍 **Query parameters** - Añadir parámetros automáticamente a la URL
- 📊 **Respuesta detallada** - Status code, headers y body de respuesta
- 🛡️ **SSL/TLS flexible** - Soporte para certificados no confiables
- 📝 **Logging detallado** - Trazabilidad completa con UUID por request

#### 💻 Interfaz web

Proporciona una interfaz web intuitiva (`/index.html`) similar a Postman para:
- Configurar requests HTTP de forma visual
- Testear APIs desde la infraestructura DENA
- Validar conectividad con servicios de administraciones
- Debugging de integraciones

#### 🔌 API REST

**Endpoint:** `POST /api/devtools/test-endpoint`

```json
{
  "method": "POST",
  "url": "https://api.administracion.com/endpoint",
  "headers": {
    "Content-Type": "application/json",
    "Authorization": "Bearer token..."
  },
  "body": "{\"key\": \"value\"}"
}
```

**Respuesta:**
```json
{
  "statusCode": 200,
  "responseBody": "...",
  "responseHeaders": {...},
  "success": true
}
```

#### ⚙️ Casos de uso

- **Testing desde infraestructura DENA** - Probar conectividad desde Tanzú hacia administraciones
- **Validación de endpoints** - Verificar que los servicios de administración son accesibles
- **Debugging de integraciones** - Diagnosticar problemas de conectividad o formato
- **Pruebas de autenticación** - Validar tokens, certificados y credenciales
- **Desarrollo y testing** - Herramienta auxiliar para equipos de desarrollo

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