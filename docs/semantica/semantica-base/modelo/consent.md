# :material-shield-check: Consentimiento (consentOid)

## Descripción

El consentimiento (o base habilitante) que respalda una petición se referencia por su **OID**. En el modelo de código, el consentimiento se transmite como un único campo `consentOid` (tipo `DN00ConsentOID`) en la base de las peticiones (`DN00InteropRequestMessageBase`).

!!! info "Presente en todas las peticiones"
    `consentOid` forma parte de la base de las peticiones de interoperabilidad, no solo de Data-Retrieve. Su presencia efectiva depende de si la operación requiere base habilitante.

---

## Atributos JSON

| Campo | Tipo | Obligatorio | Descripción |
|---|---|:---:|---|
| `consentOid` | `OID` (`DN00ConsentOID`) | :material-close: | Identificador único del consentimiento en el repositorio de consentimientos |

---

## Ejemplo

```json
{
  "consentOid": "db761b72-1634-4fb0-b7f1-3c1ebbdbb1eb"
}
```

---

## Verificación

El modelo actual solo transporta el OID del consentimiento. El detalle del consentimiento (cuándo se otorgó, vigencia, etc.) se gestiona en el repositorio de consentimientos de DENA; la API de consulta de ese repositorio está pendiente de definición.

Para más contexto sobre el ciclo de vida del consentimiento, consulta: [:octicons-arrow-right-24: Consentimientos](../consentimientos.md)

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v{{ dena.version }} · {{ dena.date }}</sub>
