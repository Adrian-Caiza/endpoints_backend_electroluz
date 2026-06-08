# POST /clients

## Descripción
Crea un nuevo cliente para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json`

## Body (obligatorios)
- `clnteemid`: id de la empresa.
- `clntetipoidentificacion`: tipo de identificación (`cedula` o `ruc`).
- `clnteidentificacion`: identificación válida según el tipo.
- `clntenombre`: nombre del cliente.
- `clntecorreo`: correo del cliente (formato válido).
- `clntedireccion`: dirección del cliente.
- `clntetelefono`: teléfono móvil (10 dígitos, ejemplo `0987654321`).

## Ejemplo (JSON)

```json
{
  "clnteemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "clntetipoidentificacion": "cedula",
  "clnteidentificacion": "1712345678",
  "clntenombre": "Juan Perez",
  "clntecorreo": "juan.perez@email.com",
  "clntedireccion": "Av. Principal y Calle 10",
  "clntetelefono": "0987654321"
}
```

## Respuesta Exitosa
`201 Created`

```json
{
  "clnteid": "9a4d1377-c64c-4fd7-bf7e-f23d2d9fd5f9",
  "clnteemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "clntetipoidentificacion": "cedula",
  "clnteidentificacion": "1712345678",
  "clntenombre": "Juan Perez",
  "clntecorreo": "juan.perez@email.com",
  "clntedireccion": "Av. Principal y Calle 10",
  "clntetelefono": "0987654321",
  "clntefchregistro": "2026-05-24T03:10:15.000Z",
  "clnteestado": "activo"
}
```

## Comportamiento relevante
- Solo permite crear clientes en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede crear clientes.
- No permite repetir `clnteidentificacion` dentro de la misma empresa.
- No permite repetir `clntecorreo` dentro de la misma empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `clnteemid` ausente: `500 Internal Server Error` con mensaje `Company id is required`.
- `clntetipoidentificacion` ausente: `500 Internal Server Error` con mensaje `Client identification type is required`.
- `clntetipoidentificacion` inválido: `500 Internal Server Error` con mensaje `Client identification type must be cedula or ruc`.
- `clnteidentificacion` ausente o inválida para el tipo: `500 Internal Server Error` con mensaje `Client identification must be valid for the selected type`.
- `clntenombre` ausente: `500 Internal Server Error` con mensaje `Client name is required`.
- `clntecorreo` ausente o inválido: `500 Internal Server Error` con mensaje `Client email must be valid`.
- `clntedireccion` ausente: `500 Internal Server Error` con mensaje `Client address is required`.
- `clntetelefono` ausente o inválido: `500 Internal Server Error` con mensaje `Client phone must be valid`.
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Intento de crear para otra empresa: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Cliente duplicado por identificación: `500 Internal Server Error` con mensaje `Client already exists with that identification`.
- Cliente duplicado por correo: `500 Internal Server Error` con mensaje `Client already exists with that email`.
