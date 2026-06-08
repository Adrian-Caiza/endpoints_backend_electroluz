# GET /branches

## Descripción
Obtiene el listado paginado de sucursales de la empresa del usuario autenticado.

## Autenticación
Requiere token JWT en el header:

`Authorization: Bearer <token>`

## Query Params (obligatorios)
- `page`: número de página (entero positivo).
- `pageSize`: cantidad de registros por página (entero positivo).

Ejemplo:

`GET /branches?page=1&pageSize=20`

## Respuesta Exitosa
`200 OK`

```json
{
  "items": [
    {
      "suid": "30d3385c-445d-4f2f-97f9-3d3f88c052f1",
      "suemid": "e2f7b231-fc3d-4010-9ec0-47ed08a34593",
      "sunombre": "Sucursal Centro",
      "sudireccion": "Av. Principal y Calle 10",
      "sucorreo": "sucursal.centro@empresa.com",
      "suidentificador": "001",
      "sufchregistro": "2026-05-19T22:10:00.000Z",
      "suestado": "activo"
    }
  ],
  "page": 1,
  "pageSize": 20,
  "totalItems": 3,
  "totalPages": 1
}
```

## Comportamiento relevante
- Solo retorna sucursales de la misma empresa del usuario autenticado.
- El usuario autenticado debe estar activo.
- La empresa del usuario autenticado debe estar activa.
- Roles permitidos para este endpoint: `jefe`, `empleado` o `administrador`.

## Errores comunes
- Token inválido o ausente: `401 Unauthorized`.
- `page` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page must be a positive integer`.
- `pageSize` inválido (no entero positivo): `500 Internal Server Error` con mensaje `Page size must be a positive integer`.
- Usuario inactivo: `500 Internal Server Error` con mensaje `User is not active`.
- Empresa inactiva: `500 Internal Server Error` con mensaje `Company is not active`.
