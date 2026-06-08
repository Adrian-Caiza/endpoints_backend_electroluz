# GET /brands/:id

## Descripción
Obtiene una marca por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la marca.

Ejemplo:

`GET /brands/5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9`

## Respuesta Exitosa
`200 OK`

```json
{
  "mrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
  "mrcemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mrcnombre": "Truper",
  "mrcfchregistro": "2026-05-21T20:30:00.000Z",
  "mrcestado": "activo"
}
```

## Comportamiento relevante
- Solo permite ver marcas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede consultar una marca.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- Marca no encontrada: `500 Internal Server Error` con mensaje `Brand not found` (comportamiento actual del servicio).
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
