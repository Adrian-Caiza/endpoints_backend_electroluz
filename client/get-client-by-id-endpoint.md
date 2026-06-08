# GET /clients/:id

## Descripción
Obtiene un cliente por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del cliente.

Ejemplo:

`GET /clients/9a4d1377-c64c-4fd7-bf7e-f23d2d9fd5f9`

## Respuesta Exitosa
`200 OK`

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
- Solo permite ver clientes de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Solo un usuario con rol `jefe` o `empleado` puede consultar un cliente.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o inválido: `400 Bad Request`.
- Cliente no encontrado: `500 Internal Server Error` con mensaje `Client not found` (comportamiento actual del servicio).
- Usuario sin rol `jefe` o `empleado`: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
