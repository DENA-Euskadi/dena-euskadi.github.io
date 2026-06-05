# PersonRef

## Descripción

Objeto para referenciar a una persona registrada en DENA por su oid o id (NIF, NIE, etc).

## Atributos JSON

| Campo | Tipo     | Obligatorio | Descripción |
|-------|----------|-------------|-------------|
| `oid` | `String` | ❌*         | Identificador interno de la persona. Obligatorio si no se incluye el campo `id` |
| `id`  | `String` | ❌*         | Identificador externo de la persona (NIF, NIE, etc). Obligatorio si no se incluye el campo `oid` |

## Ejemplo JSON

```json
{
    "id": "12345678A",
    "oid": "6AE83A0C-2202-4666-9857-3334C14663A2"
}
```