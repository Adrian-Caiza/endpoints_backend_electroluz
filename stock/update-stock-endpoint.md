# PATCH /stocks/:id

## Descripción
Actualiza un stock por su identificador, limitado a la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Path Param
- `id`: identificador del stock.

Ejemplo:

`PATCH /stocks/d5c2b3dc-1a80-46f6-b7ce-9894ea31fd87`

## Body
- `stcksuid` (obligatorio): id de la sucursal del stock.
- `stckcantidad` (opcional): nueva cantidad.
- `stckestado` (opcional): estado del stock (`activo`, `inactivo`, `eliminado`).

## Reglas importantes
- Debes enviar al menos un campo a actualizar (`stckcantidad` o `stckestado`).
- No se permite cambiar el producto del stock en este endpoint.
- `stcksuid` debe coincidir con la sucursal del stock enviado en `:id`.

## Ejemplo (JSON)

```json
{
  "stcksuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "stckcantidad": 40,
  "stckestado": "activo"
}
```

## Respuesta Exitosa
`200 OK`

```json
{
  "stckid": "d5c2b3dc-1a80-46f6-b7ce-9894ea31fd87",
  "stckemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "sucursal": {
    "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
    "sunombre": "Sucursal Centro",
    "suidentificador": "001"
  },
  "producto": {
    "prdtoid": "f8a0a2ab-9fbe-4fcf-b4d4-6888e0c4f410",
    "prdtocodigo": "PRD-0001",
    "prdtonombre": "Taladro Inalambrico 20V"
  },
  "stckcantidad": 40,
  "stckfchregistro": "2026-05-23T23:21:23.477Z",
  "stckfchactualizacion": "2026-05-24T02:43:31.452Z",
  "stckestado": "activo"
}
```

## Comportamiento relevante
- Solo permite actualizar stocks de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- Si el stock está en estado `eliminado`, no se permite actualizar.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `id` ausente o inválido: `400 Bad Request` con mensaje `Stock id is required`.
- `stcksuid` ausente o inválido: `400 Bad Request` con mensaje `Stock branch id is required`.
- `stckcantidad` inválido: `500 Internal Server Error` con mensaje `Number is required` o `Value must be a valid number`.
- `stckestado` inválido: `500 Internal Server Error` con mensaje `Stock status must be activo, inactivo or eliminado`.
- No enviar campos para actualizar: `500 Internal Server Error` con mensaje `At least one field is required to update stock`.
- Intento de actualizar un stock eliminado: `500 Internal Server Error` con mensaje `Deleted stock cannot be updated`.
- Sucursal inexistente para `stcksuid`: `500 Internal Server Error` con mensaje `Stock branch does not exist`.
- Stock no encontrado o sucursal no coincide: `500 Internal Server Error` con mensaje `Stock not found`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
