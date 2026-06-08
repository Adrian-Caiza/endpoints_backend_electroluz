# PATCH /clients/:id

## Descripción
Actualiza un cliente por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del cliente.

Ejemplo:

`PATCH /clients/9a4d1377-c64c-4fd7-bf7e-f23d2d9fd5f9`

## Body (todos opcionales)
- `clntetipoidentificacion`: tipo de identificación (`cedula` o `ruc`).
- `clnteidentificacion`: identificación válida según el tipo final del cliente.
- `clntenombre`: nombre del cliente.
- `clntecorreo`: correo del cliente (formato válido). También puede enviarse `null`.
- `clntedireccion`: dirección del cliente. También puede enviarse `null`.
- `clntetelefono`: teléfono móvil (10 dígitos, ejemplo `0987654321`). También puede enviarse `null`.
- `clnteestado`: estado del cliente (`activo`, `inactivo`, `eliminado`).

## Reglas importantes
- Debes enviar al menos un campo para actualizar.
- Si cambias `clntetipoidentificacion`, la `clnteidentificacion` resultante debe ser válida para ese tipo.
- No permite duplicar `clnteidentificacion` dentro de la misma empresa.
- No permite duplicar `clntecorreo` dentro de la misma empresa.

## Ejemplo (JSON)

```json
{
  "clntetipoidentificacion": "cedula",
  "clnteidentificacion": "1712345678",
  "clntenombre": "Juan Perez Actualizado",
  "clntecorreo": "juan.perez.actualizado@email.com",
  "clntedireccion": "Av. Amazonas y Naciones Unidas",
  "clntetelefono": "0991234567",
  "clnteestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "clnteid": "9a4d1377-c64c-4fd7-bf7e-f23d2d9fd5f9",
  "clnteemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "clntetipoidentificacion": "cedula",
  "clnteidentificacion": "1712345678",
  "clntenombre": "Juan Perez Actualizado",
  "clntecorreo": "juan.perez.actualizado@email.com",
  "clntedireccion": "Av. Amazonas y Naciones Unidas",
  "clntetelefono": "0991234567",
  "clntefchregistro": "2026-05-24T03:10:15.000Z",
  "clnteestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar clientes de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede actualizar clientes.
- Si el cliente está en estado `eliminado`, no se permite actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o inválido: `400 Bad Request`.
- `clntetipoidentificacion` inválido: `500 Internal Server Error` con mensaje `Client identification type must be cedula or ruc`.
- `clnteidentificacion` inválida para el tipo: `500 Internal Server Error` con mensaje `Client identification must be valid for the selected type`.
- `clntenombre` vacío: `500 Internal Server Error` con mensaje `Client name is required`.
- `clntecorreo` inválido: `500 Internal Server Error` con mensaje `Client email must be valid`.
- `clntedireccion` vacía: `500 Internal Server Error` con mensaje `Client address is required`.
- `clntetelefono` inválido: `500 Internal Server Error` con mensaje `Client phone must be valid`.
- `clnteestado` inválido: `500 Internal Server Error` con mensaje `Client status must be activo, inactivo or eliminado`.
- No enviar campos para actualizar: `500 Internal Server Error` con mensaje `At least one field is required to update client`.
- Intento de actualizar un cliente eliminado: `500 Internal Server Error` con mensaje `Deleted client cannot be updated`.
- Cliente duplicado por identificación: `500 Internal Server Error` con mensaje `Client already exists with that identification`.
- Cliente duplicado por correo: `500 Internal Server Error` con mensaje `Client already exists with that email`.
- Cliente no encontrado: `500 Internal Server Error` con mensaje `Client not found` (comportamiento actual del servicio).
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
