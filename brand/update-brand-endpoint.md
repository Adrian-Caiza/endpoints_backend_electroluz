# PATCH /brands/:id

## Descripción
Actualiza una marca por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la marca.

Ejemplo:

`PATCH /brands/5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9`

## Body
- `mrcnombre` (opcional): nombre de la marca.
- `mrcestado` (opcional): estado de la marca (`activo`, `inactivo`, `eliminado`).

## Ejemplo (JSON)

```json
{
  "mrcnombre": "Truper Pro",
  "mrcestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "mrcid": "5d2f2f91-e1d9-4f3f-b2ad-35f54f9500b9",
  "mrcemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "mrcnombre": "Truper Pro",
  "mrcfchregistro": "2026-05-21T20:30:00.000Z",
  "mrcestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar marcas de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede actualizar una marca.
- Si la marca está en estado `eliminado`, no se permite actualizar.
- Si se envía `mrcnombre`, se valida que no exista duplicado dentro de la misma empresa.
- Debe enviarse al menos un campo para actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `mrcnombre` vacío: `500 Internal Server Error` con mensaje `Brand name is required`.
- `mrcestado` inválido: `500 Internal Server Error` con mensaje `Brand status must be activo, inactivo or eliminado`.
- Body sin cambios efectivos: `500 Internal Server Error` con mensaje `At least one field is required to update brand`.
- Marca duplicada por nombre en la misma empresa: `500 Internal Server Error` con mensaje `Brand already exists with that name`.
- Marca no encontrada: `404 Not Found`.
- Marca eliminada: `500 Internal Server Error` con mensaje `Deleted brand cannot be updated`.
- Intento de actualización sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
