# :material-shield-check: DENAConsent

## Descripción

Objeto que contiene la referencia a la **base habilitante** (consentimiento o habilitación normativa) que respalda una petición de interoperabilidad.

!!! warning "Solo en Data-Retrieve"
    El bloque `consent` únicamente está presente en los mensajes de **recuperación de datos** (Data-Retrieve). Permite a la administración verificar que existe una base que habilita el intercambio de datos de la persona.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `consentOid` | `OID` | :material-check: | Identificador único del consentimiento en el repositorio común |
| `consentURL` | `URL` | :material-check: | URL donde la parte receptora puede encontrar y descargar los detalles del consentimiento |
| `consentData` | `Object` | :material-close: | Algunos detalles del consentimiento (cuándo se otorgó, por qué medio, hasta cuándo, etc.) |

---

## Ejemplo

```json
{
  "consent": {
    "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentURL": "https://interop.api.dena.eus/consent/db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb",
    "consentData": {
      "grantedAt": "2025-06-15T10:30:00.000Z",
      "expiresAt": "2026-06-15T10:30:00.000Z",
      "grantedVia": "DENA_APP_ENROLLMENT"
    }
  }
}
```

---

## Verificación por la administración

La administración puede, en cualquier momento, acceder a `consentURL` para:

1. Descargar todos los detalles del consentimiento
2. Obtener un **justificante firmado** emitido por el repositorio común
3. Verificar que el consentimiento sigue vigente

!!! tip "Verificación opcional"
    DENA-CORE ya verifica la existencia de la base habilitante antes de enviar la petición. La administración puede confiar en este mecanismo o verificar adicionalmente si lo considera necesario.

---

## Relación con el ciclo de vida del consentimiento

Para más detalle sobre cómo se gestionan los consentimientos en DENA, consulta: [:octicons-arrow-right-24: Consentimientos](../consentimientos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
