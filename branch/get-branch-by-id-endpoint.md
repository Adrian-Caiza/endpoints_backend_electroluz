# GET /branches/:id

## Descripción
Obtiene una sucursal por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la sucursal.

Ejemplo:

`GET /branches/30d3385c-445d-4f2f-97f9-3d3f88c052f1`

## Respuesta Exitosa
`200 OK`

```json
{
  "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "suemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "sunombre": "Sucursal Centro",
  "sudireccion": "Av. Principal y Calle 10",
  "sucorreo": "sucursal.centro@empresa.com",
  "suidentificador": "001",
  "sufchregistro": "2026-05-19T22:10:00.000Z",
  "suestado": "activo"
}
```

## Comportamiento relevante
- Solo permite ver sucursales de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- Sucursal no encontrada: `500 Internal Server Error` con mensaje `Branch not found` (comportamiento actual del servicio).
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
