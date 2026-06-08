# GET /stocks/all

## Descripción
Obtiene el listado paginado de todos los stocks de la empresa del usuario autenticado, sin filtrar por sucursal.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /stocks/all?page=1&pageSize=20`

```bash
curl -X GET "http://localhost:3000/stocks/all?page=1&pageSize=20" \
  -H "Authorization: Bearer TU_TOKEN"
```

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
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
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 1,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna stocks de la misma empresa del usuario autenticado.
- La empresa del usuario autenticado debe estar activa.
- El usuario autenticado debe estar activo.
- Roles permitidos para este endpoint: `jefe` o `empleado`.
- El identificador del producto se retorna en `producto.prdtoid` (no se repite como campo raíz).

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario sin rol permitido: `500 Internal Server Error` con mensaje `User is not jefe or empleado`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
