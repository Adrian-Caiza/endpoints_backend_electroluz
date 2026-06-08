# GET /users/:id

## Descripción
Obtiene un usuario por su identificador.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del usuario.

Ejemplo:

`GET /users/2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07`

## Respuesta Exitosa
`200 OK`

```json
{
  "usid": "2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07",
  "usemid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
  "usnombre": "Juan Perez",
  "usapodo": "jperez",
  "uscorreo": "juan.perez@empresa.com",
  "usimagen": "https://api.tudominio.com/uploads/usuarios/user.png",
  "usrol": "empleado",
  "usfchregistro": "2026-05-18T18:20:10.000Z",
  "usestado": "activo"
}
```

## Comportamiento relevante
- El backend transforma `usimagen` a URL pública completa.
- Solo retorna el usuario si pertenece a la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Los roles permitidos para este endpoint son `administrador`, `jefe` o `empleado`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request` con mensaje `User id is required`.
- Usuario no encontrado: `500 Internal Server Error` con mensaje `User not found`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
