# PATCH /users/:id

## Descripción
Actualiza los datos de un usuario y retorna el usuario con los valores actualizados.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json` (sin imagen)
- `multipart/form-data` (con imagen)

## Path Param
- `id`: identificador del usuario.

Ejemplo:

`PATCH /users/2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07`

## Body
- `usnombre` (opcional): nombre del usuario.
- `uscorreo` (opcional): correo del usuario.
- `usestado` (opcional): estado del usuario (`activo`, `inactivo`, `eliminado`).
- `usrol` (opcional): rol del usuario (`jefe`, `empleado`).
- `imagen` (opcional, archivo): imagen de perfil.

## Reglas de imagen (si se envía)
- Campo: `imagen`
- Formatos permitidos: `.png`, `.jpg`
- Tamaño máximo: `5MB`

## Ejemplo (JSON)

```json
{
  "usnombre": "Juan Perez",
  "uscorreo": "juan.perez@empresa.com",
  "usrol": "empleado"
}
```

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
- Solo permite actualizar usuarios de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Los roles permitidos para este endpoint son `jefe`, `empleado` o `administrador`.
- Si se envía `usrol` o `usestado`, solo un usuario con rol `jefe` puede cambiarlo.
- Si se envía `usrol` o `usestado`, el usuario objetivo debe tener rol `jefe` o `empleado` (no `administrador`).
- Si el usuario objetivo está en estado `eliminado`, no se permite actualizar.
- Debe enviarse al menos un campo para actualizar.
- Si se envía `uscorreo` y ya existe, se retorna error de negocio.
- El backend transforma `usimagen` a URL pública completa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request` con mensaje `User id is required`.
- Body sin campos para actualizar: `500 Internal Server Error` con mensaje `At least one field is required to update user`.
- `usnombre` ausente o inválido: `500 Internal Server Error` con mensaje `Name is required` o `Name must contain only letters and spaces`.
- `uscorreo` ausente o inválido: `500 Internal Server Error` con mensaje `Email is required` o `Email must be valid`.
- `usimagen` vacía: `500 Internal Server Error` con mensaje `User image is required`.
- `usestado` vacío: `500 Internal Server Error` con mensaje `User status is required`.
- `usestado` inválido: `500 Internal Server Error` con mensaje `User is not active`.
- `usrol` ausente o inválido: `500 Internal Server Error` con mensaje `Role is required` o `Role must be valid`.
- Imagen inválida: `500 Internal Server Error` con mensaje `Image size exceeds the allowed limit` o `Only PNG and JPG images are allowed`.
- Correo duplicado: `500 Internal Server Error` con mensaje `User already exists with that email`.
- Usuario objetivo no encontrado: `500 Internal Server Error` con mensaje `User not found`.
- Usuario objetivo eliminado: `500 Internal Server Error` con mensaje `Deleted user cannot be updated`.
- Usuario autenticado sin rol `jefe` al cambiar `usrol` o `usestado`: `500 Internal Server Error` con mensaje `User is not jefe`.
- Usuario objetivo con rol fuera de `jefe` o `empleado` al cambiar `usrol` o `usestado`: `500 Internal Server Error` con mensaje `Role must be jefe or empleado`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Usuario autenticado inactivo: `500 Internal Server Error` con mensaje `User is not active`.
