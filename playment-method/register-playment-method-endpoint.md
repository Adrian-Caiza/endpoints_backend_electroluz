# POST /playment-methods

## Descripción
Crea un nuevo método de pago para la empresa indicada en el body.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json`

## Body (obligatorios)
- `mpemid`: id de la empresa.
- `mpnombre`: nombre del método de pago.

## Ejemplo (JSON)

```json
{
  "mpemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "mpnombre": "Transferencia bancaria"
}
```

## Respuesta Exitosa
`201 Created`

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
- Solo permite crear métodos de pago en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- No permite repetir `mpnombre` dentro de la misma empresa.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `mpemid` ausente o vacío: `500 Internal Server Error` con mensaje `Company id is required`.
- `mpnombre` ausente o vacío: `500 Internal Server Error` con mensaje `Playment method name is required`.
- Empresa distinta a la del usuario autenticado: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Método duplicado por nombre: `500 Internal Server Error` con mensaje `Playment method already exists with that name`.
