# PATCH /branches/:id

## Descripción
Actualiza una sucursal por su identificador, limitada a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador de la sucursal.

Ejemplo:

`PATCH /branches/30d3385c-445d-4f2f-97f9-3d3f88c052f1`

## Body
- `sunombre` (opcional): nombre de la sucursal.
- `sudireccion` (opcional): dirección de la sucursal. También puede enviarse `null`.
- `sucorreo` (opcional): correo de la sucursal. También puede enviarse `null`.
- `suidentificador` (opcional): identificador de sucursal (exactamente 3 dígitos).
- `suestado` (opcional): estado de la sucursal (`activo`, `inactivo`, `eliminado`).

## Ejemplo (JSON)

```json
{
  "sunombre": "Sucursal Norte",
  "sudireccion": "Av. Norte 123 y Calle 8",
  "sucorreo": "norte@empresa.com",
  "suidentificador": "002",
  "suestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "suemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
  "sunombre": "Sucursal Norte",
  "sudireccion": "Av. Norte 123 y Calle 8",
  "sucorreo": "norte@empresa.com",
  "suidentificador": "002",
  "sufchregistro": "2026-05-19T22:10:00.000Z",
  "suestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar sucursales de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe tener rol `jefe` para actualizar sucursales.
- Si la sucursal está en estado `eliminado`, no se permite actualizar.
- Si se envía `suidentificador`, se valida que no exista duplicado dentro de la misma empresa.
- Debe enviarse al menos un campo para actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` inválido o ausente: `400 Bad Request`.
- `sunombre` vacío: `500 Internal Server Error` con mensaje `Branch name is required`.
- `sudireccion` vacía: `500 Internal Server Error` con mensaje `Branch address is required`.
- `suidentificador` inválido: `500 Internal Server Error` con mensaje `Branch identifier must be exactly 3 digits number`.
- `sucorreo` inválido: `500 Internal Server Error` con mensaje `Branch email must be valid`.
- `suestado` inválido: `500 Internal Server Error` con mensaje `Branch status must be activo, inactivo or eliminado`.
- Body sin cambios efectivos: `500 Internal Server Error` con mensaje `At least one field is required to update branch`.
- Sucursal no encontrada: `500 Internal Server Error` con mensaje `Branch not found`.
- Sucursal eliminada: `500 Internal Server Error` con mensaje `Deleted branch cannot be updated`.
- Intento de actualización sin rol `jefe`: `500 Internal Server Error` con mensaje `User is not jefe`.
- Identificador duplicado: `500 Internal Server Error` con mensaje `Branch already exists with that identifier`.
