# PATCH /users/:id/status

## Descripción
Actualiza el estado de un usuario.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del usuario.

Ejemplo:

`PATCH /users/2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07/status`

## Body
- `usestado`: nuevo estado del usuario.

Valores permitidos para `usestado`:
- `activo`
- `inactivo`
- `eliminado`

## Ejemplo

```json
{
  "usestado": "inactivo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "message": "User status updated"
}
```

## Comportamiento relevante
- Solo permite actualizar usuarios de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Los roles permitidos para este endpoint son `jefe`, `empleado` o `administrador`.
- Si el usuario objetivo ya está en estado `eliminado`, no se permite cambiar su estado.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request` con mensaje `User id is required`.
- `usestado` ausente: `500 Internal Server Error` con mensaje `User status is required`.
- `usestado` inválido: `500 Internal Server Error` con mensaje `User status must be activo, inactivo or eliminado`.
- Usuario no encontrado: `404 Not Found` con mensaje `User not found`.
- Usuario objetivo en estado `eliminado`: `500 Internal Server Error` con mensaje `Deleted user status cannot be changed`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Usuario autenticado inactivo: `500 Internal Server Error` con mensaje `User is not active`.
