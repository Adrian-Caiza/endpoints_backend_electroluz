# GET /medidas/:id

## Descripción
Obtiene una medida por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la medida.

Ejemplo:

`GET /medidas/7f4f7a99-f7dc-4b10-9326-9e4ec8300e89`

## Respuesta Exitosa
`200 OK`

```json
{
  "mdiaid": "7f4f7a99-f7dc-4b10-9326-9e4ec8300e89",
  "mdiaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mdianombre": "Unidad",
  "mdiaabreviatura": "UND",
  "mdiafchregistro": "2026-05-23T17:29:01.621Z",
  "mdiaestado": "activo"
}
```

## Comportamiento relevante
- Solo permite ver medidas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede consultar una medida.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- Medida no encontrada: `500 Internal Server Error` con mensaje `Medida not found` (comportamiento actual del servicio).
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
