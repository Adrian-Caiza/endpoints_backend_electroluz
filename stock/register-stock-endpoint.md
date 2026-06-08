# POST /stocks

## Descripción
Registra un stock para una sucursal y producto de una empresa.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Content-Type
- `application/json`

## Body (obligatorios)
- `stckemid`: id de la empresa.
- `stcksuid`: id de la sucursal.
- `stckprdtoid`: id del producto.
- `stckcantidad`: cantidad de stock (numero).

## Ejemplo (JSON)

```json
{
  "stckemid": "4ff4db6b-f18f-4ecd-83b3-b997fa77a01e",
  "stcksuid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
  "stckprdtoid": "f8a0a2ab-9fbe-4fcf-b4d4-6888e0c4f410",
  "stckcantidad": 25
}
```

## Respuesta Exitosa
`201 Created`

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
  "stckcantidad": 25,
  "stckfchregistro": "2026-05-23T23:21:23.477Z",
  "stckfchactualizacion": "2026-05-23T23:21:23.477Z",
  "stckestado": "activo"
}
```

## Comportamiento relevante
- Solo permite registrar stock en la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- `stcksuid` debe existir como sucursal dentro de la empresa indicada.
- `stckprdtoid` debe existir como producto dentro de la empresa indicada.
- No permite crear dos stocks para la misma combinacion `sucursal + producto`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `stckemid` ausente o vacío: `500 Internal Server Error` con mensaje `Company id is required`.
- `stcksuid` ausente o vacío: `500 Internal Server Error` con mensaje `Stock branch id is required`.
- `stckprdtoid` ausente o vacío: `500 Internal Server Error` con mensaje `Stock product id is required`.
- `stckcantidad` vacío o inválido: `500 Internal Server Error` con mensaje `Number is required` o `Value must be a valid number`.
- Intento de registrar para otra empresa distinta a la del usuario autenticado: `500 Internal Server Error` con mensaje `User cannot access another company`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
- Sucursal inexistente para la empresa indicada: `500 Internal Server Error` con mensaje `Stock branch does not exist`.
- Producto inexistente para la empresa indicada: `500 Internal Server Error` con mensaje `Stock product does not exist`.
- Stock duplicado para la misma sucursal y producto: `500 Internal Server Error` con mensaje `Stock already exists for this branch and product`.
