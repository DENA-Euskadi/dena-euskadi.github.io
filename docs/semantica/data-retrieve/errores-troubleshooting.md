# Errores y Troubleshooting

## Descripción

Guía de errores comunes al implementar el endpoint DATA-RETRIEVE y cómo resolverlos.

---

## Errores en la petición (recibidos por la administración)

### 400 — Petición malformada

| Causa | Solución |
|-------|----------|
| JSON inválido en el body | Validar que el body es JSON válido antes de procesarlo |
| Falta `context.subjectPerson.personId` | Campo obligatorio — verificar que se recibe |
| Falta `context.dataType.dataTypeId` | Campo obligatorio — verificar que se recibe |
| `dataTypeId` con valor no reconocido | Aceptar solo: `RECORDS`, `NOTICES`, `REGISTRY`, `PAYMENTS`, `SCHEDULE`, `PERSON_DATA` |

### 401 — No autorizado

| Causa | Solución |
|-------|----------|
| Token OAuth2 ausente | Verificar que la cabecera `Authorization: Bearer <token>` está presente |
| Token expirado | DENA renueva tokens automáticamente; si persiste, revisar configuración de client credentials |
| Token inválido | Verificar que el endpoint valida contra el mismo servidor de autorización configurado en DENA |

### 404 — Persona no encontrada

| Causa | Solución |
|-------|----------|
| El `personId` no existe en el sistema de la administración | Devolver HTTP 404 con body de error estándar |
| Formato de `personId` no reconocido | Aceptar DNI (8 dígitos + letra), NIE (X/Y/Z + 7 dígitos + letra) y NIF |

---

## Errores en la respuesta (devueltos por la administración)

### Estructura de error estándar

```json
{
  "context": {
    "messageType": "PERSON_FETCH_DATA",
    "messageCorrelationId": "550e8400-e29b-41d4-a716-446655440000",
    "flowDirection": "RESPONSE",
    "subjectPerson": { "personId": "12345678A" }
  },
  "data": null,
  "code": "CLIENT_ERR",
  "errorId": "PERSON_NOT_FOUND",
  "details": { "details": "Persona no encontrada en el sistema" }
}
```

| Campo | Descripción |
|-------|-------------|
| `code` | `CLIENT_ERR` (error del cliente) o `SERVER_ERR` (error del servidor) |
| `errorId` | Código de error específico definido por la administración (opcional) |
| `details.details` | Mensaje descriptivo del error |

### Códigos de estado (`code`)

| Código | Cuándo usarlo |
|--------|---------------|
| `OK` | Respuesta exitosa (incluso con lista vacía) |
| `CLIENT_ERR` | Petición malformada, persona no encontrada, parámetros inválidos |
| `SERVER_ERR` | Error interno del servidor de la administración |
| `QUEUED` | El mensaje se ha encolado para procesamiento asíncrono |

### Errores comunes de formato en `dataItems`

| Error | Síntoma en DENA | Solución |
|-------|-----------------|----------|
| Campo `type` ausente en un objeto | DENA no puede deserializar el objeto | Incluir siempre el campo `type` con el valor correcto |
| Fecha en formato incorrecto | Error de parseo | Usar ISO 8601: `2024-03-15T10:30:00Z` |
| Fecha `LocalDate` con hora | Error de parseo | Usar solo `YYYY-MM-DD` para campos de fecha sin hora |
| `LanguageTexts` con clave de idioma inválida | Texto no se muestra | Usar `SPANISH`, `BASQUE` o `ENGLISH` (mayúsculas) |
| `amountInEuro` como string | Error de tipo | Enviar como número: `45.50`, no `"45.50"` |
| `urls` como objeto en vez de array | Error de deserialización | `urls` siempre es un array, incluso con un solo elemento |
| `state` como string en expediente | Error de tipo | En expediente y registro, `state` es un objeto con `stateCode` y `description` |
| `state` como objeto en notificación | Error de tipo | En notificación, `state` es directamente un string (el código de estado) |

---

## Problemas de conectividad

| Problema | Diagnóstico | Solución |
|----------|-------------|----------|
| DENA no puede conectar al endpoint | Timeout o connection refused | Verificar que el endpoint es accesible desde la red de DENA |
| Certificado SSL inválido | Error de handshake TLS | Usar certificado válido emitido por CA reconocida |
| Respuesta demasiado lenta | Timeout (30s por defecto) | Optimizar consultas; considerar paginación si hay muchos datos |
| Respuesta demasiado grande | Error de memoria | Limitar `dataItems` a un máximo razonable (< 1000 elementos) |

---

## Problemas de datos

| Problema | Síntoma | Solución |
|----------|---------|----------|
| Expediente sin servicio ni procedimiento | Objeto rechazado por validación | `service` y `procedure` son obligatorios en expedientes |
| Pago con `forStatus: COMPLETED` pero sin `paidAt` | Inconsistencia de datos | Si el pago está completado, incluir `paymentDates.paidAt` |
| Notificación con `readedAt` pero estado `PENDING` | Inconsistencia de datos | Si tiene fecha de lectura, el estado debe ser `ACKNOWLEDGED` o `REJECTED` |
| Cita con `durationMinutes` negativo | Validación fallida | Usar 0 para hitos, valores positivos para citas con duración |
| Domiciliación sin `directDebitData` | Objeto incompleto | El bloque `directDebitData` es obligatorio para domiciliaciones |

---

## Checklist de implementación

Antes de conectar con DENA, verificar:

- [ ] El endpoint acepta `POST` con `Content-Type: application/json`
- [ ] El endpoint devuelve `Content-Type: application/json`
- [ ] Se interpreta correctamente `context.subjectPerson.personId`
- [ ] Se interpreta correctamente `context.dataType.dataTypeId`
- [ ] Todos los objetos en `dataItems` incluyen el campo `type`
- [ ] Los textos multiidioma incluyen al menos `SPANISH` y `BASQUE`
- [ ] Las fechas están en formato ISO 8601
- [ ] Los importes son números (no strings)
- [ ] Las URLs son HTTPS válidas
- [ ] Se devuelve `{ "data": { "dataItems": [] }, "code": "OK" }` cuando no hay datos
- [ ] Se devuelve HTTP 200 incluso cuando la lista está vacía
- [ ] Los códigos HTTP de error se usan correctamente (400, 401, 404, 500)
- [ ] Se usa `code: "OK"` en éxito y `code: "CLIENT_ERR"` / `code: "SERVER_ERR"` en errores
- [ ] El endpoint responde en menos de 30 segundos

---

## Contacto y soporte

Si el error persiste tras revisar esta guía, contactar con el equipo DENA proporcionando:

1. El `messageCorrelationId` de la petición
2. El código HTTP devuelto
3. El body de la respuesta (si aplica)
4. Logs del servidor con timestamp










<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.26 · 2026-06-11</sub>
