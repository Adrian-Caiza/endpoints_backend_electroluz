# PATCH /medidas/:id

## Descripción
Actualiza una medida por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la medida.

Ejemplo:

`PATCH /medidas/7f4f7a99-f7dc-4b10-9326-9e4ec8300e89`

## Body
- `mdianombre` (opcional): nombre de la medida.
- `mdiaabreviatura` (opcional): abreviatura de la medida.
- `mdiaestado` (opcional): estado de la medida (`activo`, `inactivo`, `eliminado`).

## Ejemplo (JSON)

```json
{
  "mdianombre": "Unidad Comercial",
  "mdiaabreviatura": "UND",
  "mdiaestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "mdiaid": "7f4f7a99-f7dc-4b10-9326-9e4ec8300e89",
  "mdiaemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mdianombre": "Unidad Comercial",
  "mdiaabreviatura": "UND",
  "mdiafchregistro": "2026-05-23T17:29:01.621Z",
  "mdiaestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar medidas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede actualizar una medida.
- Si la medida está en estado `eliminado`, no se permite actualizar.
- Si se envía `mdianombre`, se valida que no exista duplicado dentro de la misma empresa.
- Si se envía `mdiaabreviatura`, se valida que no exista duplicado dentro de la misma empresa.
- Debe enviarse al menos un campo para actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `mdianombre` vacío: `500 Internal Server Error` con mensaje `Medida name is required`.
- `mdiaabreviatura` vacía: `500 Internal Server Error` con mensaje `Medida abbreviation is required`.
- `mdiaestado` inválido: `500 Internal Server Error` con mensaje `Medida status must be activo, inactivo or eliminado`.
- Body sin cambios efectivos: `500 Internal Server Error` con mensaje `At least one field is required to update medida`.
- Medida duplicada por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Medida already exists with that name`.
- Medida duplicada por abreviatura en la misma empresa: `500 Internal Server Error` con mensaje `Medida already exists with that abbreviation`.
- Medida no encontrada: `404 Not Found`.
- Medida eliminada: `500 Internal Server Error` con mensaje `Deleted medida cannot be updated`.
- Intento de actualización sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
