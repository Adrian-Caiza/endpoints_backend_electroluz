# PATCH /companies/:id/status

## Descripción
Actualiza el estado de una empresa.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la empresa.

Ejemplo:

`PATCH /companies/0f4c5bde-9a11-4c76-9b6a-2e0b58e9bc2a/status`

## Body
- `emestado`: nuevo estado de la empresa.

Valores permitidos para `emestado`:
- `activo`
- `inactivo`
- `eliminado`

## Ejemplo

```json
{
  "emestado": "inactivo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "message": "Company status updated"
}
```

## Comportamiento relevante
- Solo un usuario con rol `administrador` puede actualizar el estado de una empresa.
- El usuario debe estar activo.
- El usuario debe tener rol `administrador`.
- La empresa del usuario debe estar activa y marcada como empresa padre.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `emestado` inválido o ausente: `500 Internal Server Error` con mensaje `Company status must be activo, inactivo or eliminado`.
- Empresa no encontrada: `404 Not Found`.
- Usuario sin rol `administrador`: `500 Internal Server Error` con mensaje `User is not admin`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa autenticada no activa: `500 Internal Server Error` con mensaje `Company is not active`.
- Empresa autenticada no es empresa padre: `500 Internal Server Error` con mensaje `Company is not parent`.
- Empresa objetivo eliminada: `500 Internal Server Error` con mensaje `Deleted company status cannot be changed`.
