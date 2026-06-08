# PATCH /categories/:id

## Descripción
Actualiza una categoría por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la categoría.

Ejemplo:

`PATCH /categories/3c871a9e-0f6b-4ac9-a18f-0ecfe8b96d90`

## Body
- `ctgnombre` (opcional): nombre de la categoría.
- `ctgriadescripcion` (opcional): descripción de la categoría. También puede enviarse `null`.
- `ctgriaestado` (opcional): estado de la categoría (`activo`, `inactivo`, `eliminado`).

## Ejemplo (JSON)

```json
{
  "ctgnombre": "Herramientas Eléctricas",
  "ctgriadescripcion": "Productos y equipos eléctricos",
  "ctgriaestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "ctgriaid": "3c871a9e-0f6b-4ac9-a18f-0ecfe8b96d90",
  "ctgriaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "ctgnombre": "Herramientas Eléctricas",
  "ctgriadescripcion": "Productos y equipos eléctricos",
  "ctgriafchregistro": "2026-05-21T18:40:00.000Z",
  "ctgriaestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar categorías de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede actualizar una categoría.
- Si la categoría está en estado `eliminado`, no se permite actualizar.
- Si se envía `ctgnombre`, se valida que no exista duplicado dentro de la misma empresa.
- Debe enviarse al menos un campo para actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `ctgnombre` vacío: `500 Internal Server Error` con mensaje `Category name is required`.
- `ctgriaestado` inválido: `500 Internal Server Error` con mensaje `Category status must be activo, inactivo or eliminado`.
- `ctgriadescripcion` inválida (vacía): `500 Internal Server Error` con mensaje `Category description is required`.
- Body sin cambios efectivos: `500 Internal Server Error` con mensaje `At least one field is required to update category`.
- Categoría duplicada por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Category already exists with that name`.
- Categoría no encontrada: `404 Not Found`.
- Categoría eliminada: `500 Internal Server Error` con mensaje `Deleted category cannot be updated`.
- Intento de actualización sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
