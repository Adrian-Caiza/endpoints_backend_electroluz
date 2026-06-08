# PATCH /playment-methods/:id

## Descripción
Actualiza los datos de un método de pago por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json`

## Path Param
- `id`: identificador del método de pago.

Ejemplo:

`PATCH /playment-methods/2b2f66f5-94ad-49ff-8d43-9d7f56e8c11b`

## Body (opcionales)
- `mpnombre`: nuevo nombre del método de pago.
- `mpestado`: nuevo estado (`activo`, `inactivo`, `eliminado`).

Debes enviar al menos un campo.

## Ejemplo (JSON)

```json
{
  "mpnombre": "Tarjeta de crédito",
  "mpestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "mpid": "2b2f66f5-94ad-49ff-8d43-9d7f56e8c11b",
  "mpemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "mpnombre": "Tarjeta de crédito",
  "mpfchregistro": "2026-05-24T03:15:11.245Z",
  "mpestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar métodos de pago de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- No permite repetir `mpnombre` dentro de la misma empresa.
- Si el método de pago está en estado `eliminado`, no permite actualizarlo.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o inválido: `400 Bad Request` con mensaje `Playment method id is required`.
- Body vacío o sin cambios aplicables: `500 Internal Server Error` con mensaje `At least one field is required to update playment method`.
- `mpnombre` vacío: `500 Internal Server Error` con mensaje `Playment method name is required`.
- `mpestado` inválido: `500 Internal Server Error` con mensaje `Playment method status must be activo, inactivo or eliminado`.
- Nombre duplicado en la empresa: `500 Internal Server Error` con mensaje `Playment method already exists with that name`.
- Método de pago en estado `eliminado`: `500 Internal Server Error` con mensaje `Deleted playment method cannot be updated`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Método de pago no encontrado: `404 Not Found` con mensaje `Playment method not found`.
