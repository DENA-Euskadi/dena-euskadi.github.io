# DataTypeRef

## Descripción

Objeto para referenciar a un tipo de dato gestionado por DENA. Ej: Expediente

## Atributos JSON

| Campo | Tipo     | Obligatorio | Descripción |
|-------|----------|-------------|-------------|
| `oid` | `String` | ❌*         | Identificador interno del tipo de dato. Obligatorio si no se incluye el campo `id` |
| `id`  | `String` | ❌*         | Identificador textual del tipo de dato. Obligatorio si no se incluye el campo `oid` |

## Ejemplo JSON

```json
{
    "id": "administrativeNotice",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```

<!-- DENA-DOC-FOOTER -->
---
<sub>DENA Docs v0.3.25 · 2026-06-10</sub>
