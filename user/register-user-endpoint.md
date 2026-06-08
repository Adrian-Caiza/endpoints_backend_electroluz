# POST /users

## Descripción
Registra un nuevo usuario.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json` (sin imagen)
- `multipart/form-data` (con imagen)

## Body
- `usemid`: id de la empresa a la que pertenecerá el usuario.
- `usnombre`: nombre del usuario.
- `usapodo`: apodo (nickname) del usuario.
- `uscorreo`: correo del usuario.
- `uspassword`: contraseña (mínimo 8 caracteres).
- `usrol`: rol del usuario (`administrador`, `jefe`, `empleado`).
- `imagen` (opcional, archivo): imagen de perfil.

## Reglas de imagen (si se envía)
- Campo: `imagen`
- Formatos permitidos: `.png`, `.jpg`
- Tamaño máximo: `5MB`
- Si no se envía, se usa imagen por defecto.

## Ejemplo (JSON)

`POST /users`

```json
{
  "usemid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
  "usnombre": "Juan Perez",
  "usapodo": "jperez",
  "uscorreo": "juan.perez@empresa.com",
  "uspassword": "secreto123",
  "usrol": "empleado"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "usid": "2a6eb0a9-10e1-49fb-a2f3-1e5c580f5a07",
  "usemid": "0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a",
  "usnombre": "Juan Perez",
  "usapodo": "jperez",
  "uscorreo": "juan.perez@empresa.com",
  "usimagen": "https://api.tudominio.com/uploads/usuarios/user.png",
  "usrol": "empleado",
  "usfchregistro": "2026-05-23T17:29:01.621Z",
  "usestado": "activo"
}
```

## Comportamiento relevante
- Si no se envía imagen, el backend guarda la ruta por defecto `/uploads/usuarios/user.png`.
- El backend transforma `usimagen` a URL pública completa.
- Si el usuario autenticado crea dentro de su misma empresa, solo puede hacerlo si su rol es `jefe`.
- En creación dentro de la misma empresa no se permite crear usuarios con rol `administrador`.
- Si el usuario autenticado crea para otra empresa, debe ser `administrador` y su empresa autenticada debe ser empresa padre.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `usemid` ausente o vacío: `500 Internal Server Error` con mensaje `Company id is required`.
- `usnombre` ausente o inválido: `500 Internal Server Error` con mensaje `Name is required` o `Name must contain only letters and spaces`.
- `usapodo` ausente o vacío: `500 Internal Server Error` con mensaje `Nickname is required`.
- `uscorreo` ausente o inválido: `500 Internal Server Error` con mensaje `Email is required` o `Email must be valid`.
- `uspassword` ausente o inválida: `500 Internal Server Error` con mensaje `Password is required` o `Password must be at least 8 characters`.
- `usrol` ausente o inválido: `500 Internal Server Error` con mensaje `Role is required` o `Role must be valid`.
- Imagen inválida: `500 Internal Server Error` con mensaje `Image size exceeds the allowed limit` o `Only PNG and JPG images are allowed`.
- Empresa autenticada inexistente: `500 Internal Server Error` con mensaje `Company code is not invalid`.
- Empresa destino inexistente: `500 Internal Server Error` con mensaje `Company does not exist`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Usuario autenticado inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Crear para otra empresa sin permisos: `500 Internal Server Error` con mensaje `User is not admin` o `Company is not parent`.
- Crear en la misma empresa sin rol `jefe`: `500 Internal Server Error` con mensaje `User is not jefe`.
- Crear rol `administrador` dentro de la misma empresa: `500 Internal Server Error` con mensaje `Role must be jefe or empleado`.
- Correo ya registrado: `500 Internal Server Error` con mensaje `User already exists with that email`.
- Apodo ya registrado en la empresa: `500 Internal Server Error` con mensaje `User already exists with that nickname`.
