# PATCH /users/:id/password

## Descripción
Actualiza la contraseña de un usuario y no retorna el hash actualizado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del usuario.

## Body (obligatorio)
- `uspassword`: nueva contraseña del usuario (mínimo 8 caracteres).

Ejemplo:

`PATCH /users/2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07/password`

```json
{
  "uspassword": "nuevaClave123"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "message": "User password updated"
}
```

## Comportamiento relevante
- Solo permite actualizar contraseñas dentro de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Solo un usuario con rol `jefe` puede ejecutar este endpoint.
- La contraseña se valida con mínimo de 8 caracteres y se almacena cifrada.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o inválido: `400 Bad Request` con mensaje `User id is required`.
- `uspassword` ausente o inválida: `500 Internal Server Error` con mensaje `Password is required` o `Password must be at least 8 characters`.
- Usuario sin rol `jefe`: `500 Internal Server Error` con mensaje `User is not jefe`.
- Usuario no encontrado: `404 Not Found` con mensaje `User not found`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Usuario autenticado inactivo: `500 Internal Server Error` con mensaje `User is not active`.
