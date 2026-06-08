# DELETE /configs/:configKey

## Descripción
Elimina una configuración usando su clave.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `configKey`: clave de configuración.

## Query Params (obligatorios)
- `companyId`: id de la empresa donde se eliminará la clave.

Ejemplo:

`DELETE /configs/iva_porcentaje?companyId=4ff4db6b-f18f-4ecd-83b3-b997fa77a01e`

## Respuesta Exitosa
`200 OK`

```json
{
  "cfclave": "iva_porcentaje"
}
```

## Comportamiento relevante
- Solo permite eliminar configuraciones a un usuario con rol `administrador`.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe ser empresa padre (`empadre = true`).
- La respuesta solo devuelve la clave eliminada.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `configKey` ausente: `400 Bad Request`.
- `companyId` ausente: `400 Bad Request`.
- Usuario sin rol `administrador`: `User is not admin`.
- Configuración no encontrada: `404 Not Found`.
