# GET /categories/:id

## Descripción
Obtiene una categoría por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la categoría.

Ejemplo:

`GET /categories/3c871a9e-0f6b-4ac9-a18f-0ecfe8b96d90`

## Respuesta Exitosa
`200 OK`

```json
{
  "ctgriaid": "3c871a9e-0f6b-4ac9-a18f-0ecfe8b96d90",
  "ctgriaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "ctgnombre": "Herramientas Manuales",
  "ctgriadescripcion": "Productos para uso manual en ferretería",
  "ctgriafchregistro": "2026-05-21T18:40:00.000Z",
  "ctgriaestado": "activo"
}
```

## Comportamiento relevante
- Solo permite ver categorías de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Solo un usuario con rol `jefe` o `empleado` puede consultar una categoría.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- Categoría no encontrada: `500 Internal Server Error` con mensaje `Category not found` (comportamiento actual del servicio).
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
