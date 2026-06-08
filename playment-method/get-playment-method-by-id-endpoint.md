# GET /playment-methods/:id

## Descripción
Obtiene un método de pago por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del método de pago.

Ejemplo:

`GET /playment-methods/2b2f66f5-94ad-49ff-8d43-9d7f56e8c11b`

## Respuesta Exitosa
`200 OK`

```json
{
  "mpid": "2b2f66f5-94ad-49ff-8d43-9d7f56e8c11b",
  "mpemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "mpnombre": "Transferencia bancaria",
  "mpfchregistro": "2026-05-24T03:15:11.245Z",
  "mpestado": "activo"
}
```

## Comportamiento relevante
- Solo permite ver métodos de pago de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o inválido: `400 Bad Request` con mensaje `Playment method id is required`.
- Método de pago no encontrado: `500 Internal Server Error` con mensaje `Playment method not found`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
