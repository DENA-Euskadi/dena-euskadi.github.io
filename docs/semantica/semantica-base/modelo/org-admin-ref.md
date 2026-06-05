# OrgAdminRef

## Descripción

Objeto para referenciar a una administración por su oid o id.

## Atributos JSON

| Campo | Tipo     | Obligatorio | Descripción |
|-------|----------|-------------|-------------|
| `oid` | `String` | ❌*         | Identificador interno de la administración. Obligatorio si no se incluye el campo `id` |
| `id`  | `String` | ❌*         | Identificador textual de la administración. Obligatorio si no se incluye el campo `oid` |

## Ejemplo JSON

```json
{
    "id": "admin-A414",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```